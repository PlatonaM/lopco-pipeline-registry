#### /pipelines

**GET**

_List pipeline IDs._

    # Example

    curl http://host:8000/pipeline-registry/pipelines
    [
        "783c1c1f-7680-4420-8a0c-9e00752ee22b"
    ]

**POST**

_Add new pipeline._

    # Example

    cat pipeline_data.json
    {
        "name": "Test Pipeline 2",
        "stages": [
            {
                "id": "01",
                "worker": {
                    "id": "1567e155-51c6-4f0b-a898-842c737f1b34",
                    "name": "Convert xlsx to csv",
                    "description": null,
                    "image": "xlsx-to-csv-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "xlsx_file",
                                "media_type": "application/vnd.ms-excel",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "csv_file",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "configs": {
                        "delimiter": ";"
                    }
                },
                "input_map": {
                    "xlsx_file": "init_source"
                }
            },
            {
                "id": "02",
                "worker": {
                    "id": "04e6b617-fff1-41bb-a50c-8c2a2c0413e5",
                    "name": "Trim column from csv",
                    "description": null,
                    "image": "trim-csv-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "input_csv",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "output_csv",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "configs": {
                        "delimiter": ";",
                        "column_num": 2
                    }
                },
                "input_map": {
                    "input_csv": "csv_file"
                }
            },
            {
                "id": "03",
                "worker": {
                    "id": "004894dc-bb03-4649-92c4-6b184c30c594",
                    "name": "Split on unique",
                    "description": null,
                    "image": "split-on-unique-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "source_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "unique_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "result_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "configs": {
                        "column": "sensor",
                        "delimiter": ";"
                    }
                },
                "input_map": {
                    "source_table": "output_csv"
                }
            },
            {
                "id": "04",
                "worker": {
                    "id": "9a84f6ad-2ba6-49b3-b931-b7629c34be9c",
                    "name": "Upload to platform",
                    "description": null,
                    "image": "platform-upload-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "source_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": null,
                    "configs": {
                        "user": "test_user",
                        "password": "test_pw",
                        "machine_id": "735d39eb6dc94acdadc9d019ff54fb1f"
                    }
                },
                "input_map": {
                    "service_id": "unique_id",
                    "source_table": "result_table"
                }
            }
        ]
    }

    curl \
    -d @pipeline_data.json \
    -H 'Content-Type: application/json' \
    -X POST http://host:8000/pipeline-registry/pipelines
    {
        "resource": "a34f4ba9-13ad-4ab5-a7e8-d23157513466"
    }

----

#### /pipelines/{pipeline-id}

**GET**

_Retrieve pipeline data._

    # Example

    curl http://host:8000/pipeline-registry/pipelines/783c1c1f-7680-4420-8a0c-9e00752ee22b
    {
        "name": "Test Pipeline",
        "stages": [
            {
                "id": "01",
                "worker": {
                    "id": "004894dc-bb03-4649-92c4-6b184c30c594",
                    "name": "Split on unique",
                    "description": null,
                    "image": "split-on-unique-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "source_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "unique_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "result_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "configs": {
                        "column": "sensor",
                        "delimiter": ";"
                    }
                },
                "input_map": {
                    "source_table": "init_source"
                }
            },
            {
                "id": "02",
                "worker": {
                    "id": "9a84f6ad-2ba6-49b3-b931-b7629c34be9c",
                    "name": "Upload to platform",
                    "description": null,
                    "image": "platform-upload-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "source_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": null,
                    "configs": {
                        "user": "test_user",
                        "password": "test_pw",
                        "machine_id": "3f2c8eccc5064346b97b70785c95a729"
                    }
                },
                "input_map": {
                    "service_id": "unique_id",
                    "source_table": "result_table"
                }
            }
        ]
    }


**PUT**

_Update a pipeline._

    # Example

    cat pipeline_data.json
    {
        "name": "Test Pipeline",
        "stages": [
            {
                "id": "01",
                "worker": {
                    "id": "004894dc-bb03-4649-92c4-6b184c30c594",
                    "name": "Split on unique",
                    "description": null,
                    "image": "split-on-unique-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "source_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "unique_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "result_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "configs": {
                        "column": "sensor",
                        "delimiter": ";"
                    }
                },
                "input_map": {
                    "source_table": "init_source"
                }
            },
            {
                "id": "02",
                "worker": {
                    "id": "9a84f6ad-2ba6-49b3-b931-b7629c34be9c",
                    "name": "Upload to platform",
                    "description": null,
                    "image": "platform-upload-worker",
                    "data_cache_path": "/data_cache",
                    "input": {
                        "type": "single",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "source_table",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": null,
                    "configs": {
                        "user": "user_a",
                        "password": "1234",
                        "machine_id": "3f2c8eccc5064346b97b70785c95a729"
                    }
                },
                "input_map": {
                    "service_id": "unique_id",
                    "source_table": "result_table"
                }
            }
        ]
    }

    curl \
    -d @machine_data.json \
    -H 'Content-Type: application/json' \
    -X PUT http://host:8000/pipeline-registry/pipelines/783c1c1f-7680-4420-8a0c-9e00752ee22b

**DELETE**

_Remove a pipeline_

    # Example

    curl -X DELETE http://host:8000/pipeline-registry/pipelines/783c1c1f-7680-4420-8a0c-9e00752ee22b
