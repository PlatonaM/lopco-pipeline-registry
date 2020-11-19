### Pipeline Data Structure

    {
        "name": <string>,
        "stages": {
            "0": {
                "worker": {
                    "id": <string>,
                    "name": <string>,
                    "image": <string>,
                    "data_cache_path": <string>,
                    "description": <string>,
                    "configs": {
                        <string>: <string>,
                        ...
                    },
                    "input": {
                        "type": <string>("single" / "multiple"),
                        "fields": [
                            {
                                "name": <string>,
                                "media_type": <string>,
                                "is_file": <boolean>
                            },
                            ...
                        ]
                    },
                    "output": {
                        "type": <string>("single" / "multiple"),
                        "fields": [
                            {
                                "name": <string>,
                                "media_type": <string>,
                                "is_file": <boolean>
                            },
                            ...
                        ]
                    }
                },
                "description": <string>,
                "input_map": {
                    <string>: <string>,
                    ...
                }
            },
            "1": {
                ...
            },
            ...
        }
    }

### API

#### /pipelines

**GET**

_List pipelines._

    # Example

    curl http://<host>/pipelines
    {
        "677f99d4-2ec7-450c-add2-8b7c5f7f171c": "{\"name\": \"No upload test\", \"stages\": {\"0\": {\"worker\": {\"name\": \"XLSX to CSV\", \"image\": \"platonam/lopco-xlsx-to-csv-worker:dev\", \"data_cache_path\": \"/data_cache\", \"description\": \"Convert a Microsoft Excel Open XML Spreadsheet file to Comma-Separated Values.\", \"configs\": {\"delimiter\": \";\"}, \"input\": {\"type\": \"single\", \"fields\": [{\"name\": \"xlsx_file\", \"media_type\": \"application/vnd.ms-excel\", \"is_file\": true}]}, \"output\": {\"type\": \"single\", \"fields\": [{\"name\": \"csv_file\", \"media_type\": \"text/csv\", \"is_file\": true}, {\"name\": \"line_count\", \"media_type\": \"text/plain\", \"is_file\": false}]}, \"id\": \"1567e155-51c6-4f0b-a898-842c737f1b34\"}, \"description\": \"\", \"input_map\": {\"xlsx_file\": \"init_source\"}}, \"1\": {\"worker\": {\"name\": \"Trim CSV\", \"image\": \"platonam/lopco-trim-csv-worker:dev\", \"data_cache_path\": \"/data_cache\", \"description\": \"Trim a column from a Comma-Separated Values file.\", \"configs\": {\"delimiter\": \";\", \"column_num\": \"2\"}, \"input\": {\"type\": \"single\", \"fields\": [{\"name\": \"input_csv\", \"media_type\": \"text/csv\", \"is_file\": true}]}, \"output\": {\"type\": \"single\", \"fields\": [{\"name\": \"output_csv\", \"media_type\": \"text/csv\", \"is_file\": true}, {\"name\": \"line_count\", \"media_type\": \"text/plain\", \"is_file\": false}]}, \"id\": \"04e6b617-fff1-41bb-a50c-8c2a2c0413e5\"}, \"description\": \"\", \"input_map\": {\"input_csv\": \"csv_file\"}}, \"2\": {\"worker\": {\"name\": \"Split CSV\", \"image\": \"platonam/lopco-split-csv-worker:dev\", \"data_cache_path\": \"/data_cache\", \"description\": \"Split a Comma-Separated Values file into multiple unique files.\", \"configs\": {\"column\": \"sensor\", \"delimiter\": \";\"}, \"input\": {\"type\": \"single\", \"fields\": [{\"name\": \"source_table\", \"media_type\": \"text/csv\", \"is_file\": true}]}, \"output\": {\"type\": \"multiple\", \"fields\": [{\"name\": \"unique_id\", \"media_type\": \"text/plain\", \"is_file\": false}, {\"name\": \"result_table\", \"media_type\": \"text/csv\", \"is_file\": true}, {\"name\": \"line_count\", \"media_type\": \"text/plain\", \"is_file\": false}]}, \"id\": \"004894dc-bb03-4649-92c4-6b184c30c594\"}, \"description\": \"\", \"input_map\": {\"source_table\": \"output_csv\"}}}}"
    }

**POST**

_Add new pipeline._

    # Example

    cat pipeline_data.json
    {
        "name": "Demo Pipeline",
        "stages": {
            "0": {
                "worker": {
                    "name": "XLSX to CSV",
                    "image": "platonam/lopco-xlsx-to-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Convert a Microsoft Excel Open XML Spreadsheet file to Comma-Separated Values.",
                    "configs": {
                        "delimiter": ";"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "1567e155-51c6-4f0b-a898-842c737f1b34"
                },
                "description": "",
                "input_map": {
                    "xlsx_file": "init_source"
                }
            },
            "1": {
                "worker": {
                    "name": "Trim CSV",
                    "image": "platonam/lopco-trim-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Trim a column from a Comma-Separated Values file.",
                    "configs": {
                        "delimiter": ";",
                        "column_num": "2"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "04e6b617-fff1-41bb-a50c-8c2a2c0413e5"
                },
                "description": "remove index",
                "input_map": {
                    "input_csv": "csv_file"
                }
            },
            "2": {
                "worker": {
                    "name": "Split CSV",
                    "image": "platonam/lopco-split-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Split a Comma-Separated Values file into multiple unique files.",
                    "configs": {
                        "column": "sensor",
                        "delimiter": ";"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "004894dc-bb03-4649-92c4-6b184c30c594"
                },
                "description": "",
                "input_map": {
                    "source_table": "output_csv"
                }
            },
            "3": {
                "worker": {
                    "name": "Upload CSV to platform",
                    "image": "platonam/lopco-upload-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "MQTT upload worker.",
                    "configs": {
                        "usr": "user",
                        "pw": "password",
                        "mqtt_server": "mqtt.host.com",
                        "mqtt_port": "1883",
                        "mqtt_keepalive": "15",
                        "delimiter": ";"
                    },
                    "input": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "source_file",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": true
                            },
                            {
                                "name": "sent_messages",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "9a84f6ad-2ba6-49b3-b931-b7629c34be9c"
                },
                "description": "",
                "input_map": {
                    "service_id": "unique_id",
                    "source_file": "result_table"
                }
            }
        }
    }

    curl \
    -d @pipeline_data.json \
    -H 'Content-Type: application/json' \
    -X POST http://<host>/pipelines
    {
        "resource": "a34f4ba9-13ad-4ab5-a7e8-d23157513466"
    }

----

#### /pipelines/{pipeline-id}

**GET**

_Retrieve pipeline data._

    # Example

    curl http://<host>/pipelines/a34f4ba9-13ad-4ab5-a7e8-d23157513466
    {
        "name": "Demo Pipeline",
        "stages": {
            "0": {
                "worker": {
                    "name": "XLSX to CSV",
                    "image": "platonam/lopco-xlsx-to-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Convert a Microsoft Excel Open XML Spreadsheet file to Comma-Separated Values.",
                    "configs": {
                        "delimiter": ";"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "1567e155-51c6-4f0b-a898-842c737f1b34"
                },
                "description": "",
                "input_map": {
                    "xlsx_file": "init_source"
                }
            },
            "1": {
                "worker": {
                    "name": "Trim CSV",
                    "image": "platonam/lopco-trim-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Trim a column from a Comma-Separated Values file.",
                    "configs": {
                        "delimiter": ";",
                        "column_num": "2"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "04e6b617-fff1-41bb-a50c-8c2a2c0413e5"
                },
                "description": "remove index",
                "input_map": {
                    "input_csv": "csv_file"
                }
            },
            "2": {
                "worker": {
                    "name": "Split CSV",
                    "image": "platonam/lopco-split-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Split a Comma-Separated Values file into multiple unique files.",
                    "configs": {
                        "column": "sensor",
                        "delimiter": ";"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "004894dc-bb03-4649-92c4-6b184c30c594"
                },
                "description": "",
                "input_map": {
                    "source_table": "output_csv"
                }
            },
            "3": {
                "worker": {
                    "name": "Upload CSV to platform",
                    "image": "platonam/lopco-upload-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "MQTT upload worker.",
                    "configs": {
                        "usr": "user",
                        "pw": "password",
                        "mqtt_server": "mqtt.host.com",
                        "mqtt_port": "1883",
                        "mqtt_keepalive": "15",
                        "delimiter": ";"
                    },
                    "input": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "source_file",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": true
                            },
                            {
                                "name": "sent_messages",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "9a84f6ad-2ba6-49b3-b931-b7629c34be9c"
                },
                "description": "",
                "input_map": {
                    "service_id": "unique_id",
                    "source_file": "result_table"
                }
            }
        }
    }


**PUT**

_Update a pipeline._

    # Example

    cat pipeline_data.json
    {
        "name": "Demo Pipeline",
        "stages": {
            "0": {
                "worker": {
                    "name": "XLSX to CSV",
                    "image": "platonam/lopco-xlsx-to-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Convert a Microsoft Excel Open XML Spreadsheet file to Comma-Separated Values.",
                    "configs": {
                        "delimiter": ";"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "1567e155-51c6-4f0b-a898-842c737f1b34"
                },
                "description": "",
                "input_map": {
                    "xlsx_file": "init_source"
                }
            },
            "1": {
                "worker": {
                    "name": "Trim CSV",
                    "image": "platonam/lopco-trim-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Trim a column from a Comma-Separated Values file.",
                    "configs": {
                        "delimiter": ";",
                        "column_num": "2"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "04e6b617-fff1-41bb-a50c-8c2a2c0413e5"
                },
                "description": "remove index",
                "input_map": {
                    "input_csv": "csv_file"
                }
            },
            "2": {
                "worker": {
                    "name": "Split CSV",
                    "image": "platonam/lopco-split-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "Split a Comma-Separated Values file into multiple unique files.",
                    "configs": {
                        "column": "sensor",
                        "delimiter": ";"
                    },
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
                            },
                            {
                                "name": "line_count",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "004894dc-bb03-4649-92c4-6b184c30c594"
                },
                "description": "",
                "input_map": {
                    "source_table": "output_csv"
                }
            },
            "3": {
                "worker": {
                    "name": "Upload CSV to platform",
                    "image": "platonam/lopco-upload-csv-worker:dev",
                    "data_cache_path": "/data_cache",
                    "description": "MQTT upload worker.",
                    "configs": {
                        "usr": "user",
                        "pw": "password",
                        "mqtt_server": "mqtt.host.com",
                        "mqtt_port": "1883",
                        "mqtt_keepalive": "15",
                        "delimiter": ";"
                    },
                    "input": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": false
                            },
                            {
                                "name": "source_file",
                                "media_type": "text/csv",
                                "is_file": true
                            }
                        ]
                    },
                    "output": {
                        "type": "multiple",
                        "fields": [
                            {
                                "name": "service_id",
                                "media_type": "text/plain",
                                "is_file": true
                            },
                            {
                                "name": "sent_messages",
                                "media_type": "text/plain",
                                "is_file": false
                            }
                        ]
                    },
                    "id": "9a84f6ad-2ba6-49b3-b931-b7629c34be9c"
                },
                "description": "",
                "input_map": {
                    "service_id": "unique_id",
                    "source_file": "result_table"
                }
            }
        }
    }

    curl \
    -d @pipeline_data.json \
    -H 'Content-Type: application/json' \
    -X PUT http://<host>/pipelines/a34f4ba9-13ad-4ab5-a7e8-d23157513466

**DELETE**

_Remove a pipeline_

    # Example

    curl -X DELETE http://<host>/pipelines/a34f4ba9-13ad-4ab5-a7e8-d23157513466
