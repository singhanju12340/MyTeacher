---
Creation Time: Wednesday, April 9th 2025
Modified Time: Wednesday, April 9th 2025
---
```
{
  "drp_cp_carbravo_autosuggest_index" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "displayText" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "filters" : {
          "properties" : {
            "bodyType" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "cash" : {
              "properties" : {
                "includeNull" : {
                  "type" : "boolean"
                },
                "max" : {
                  "type" : "float"
                },
                "min" : {
                  "type" : "float"
                }
              }
            },
            "convenienceFeatures" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "exteriorColor" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "make" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "mileage" : {
              "properties" : {
                "includeNull" : {
                  "type" : "boolean"
                },
                "max" : {
                  "type" : "float"
                }
              }
            },
            "model" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "premiumFeatures" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "safetyDriveFeatures" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "technologyFeatures" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "utilityFeatures" : {
              "properties" : {
                "operator" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "values" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            },
            "year" : {
              "properties" : {
                "includeNull" : {
                  "type" : "boolean"
                },
                "max" : {
                  "type" : "float"
                },
                "min" : {
                  "type" : "float"
                }
              }
            }
          }
        },
        "highlightedFacet" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "img" : {
          "type" : "boolean"
        },
        "program" : {
          "type" : "keyword",
          "fields" : {
            "text" : {
              "type" : "text"
            }
          }
        },
        "searchFields" : {
          "type" : "text",
          "analyzer" : "my_ngram_analyzer",
          "search_analyzer" : "whitespace"
        },
        "templateId" : {
          "type" : "keyword",
          "fields" : {
            "text" : {
              "type" : "text"
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "max_ngram_diff" : "24",
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "drp_cp_carbravo_autosuggest_index",
        "creation_date" : "1656593236557",
        "analysis" : {
          "analyzer" : {
            "my_ngram_analyzer" : {
              "filter" : [
                "lowercase"
              ],
              "char_filter" : [
                "html_strip"
              ],
              "type" : "custom",
              "tokenizer" : "my_ngram_tokenizer"
            }
          },
          "tokenizer" : {
            "my_ngram_tokenizer" : {
              "token_chars" : [
                "letter",
                "digit",
                "symbol"
              ],
              "min_gram" : "2",
              "type" : "edge_ngram",
              "max_gram" : "20"
            }
          }
        },
        "number_of_replicas" : "1",
        "uuid" : "gFzmjq1tQB6d2WdEdbTrBA",
        "version" : {
          "created" : "7100099"
        }
      }
    }
  }
}
```