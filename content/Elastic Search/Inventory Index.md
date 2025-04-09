---
Creation Time: Wednesday, April 9th 2025
Modified Time: Wednesday, April 9th 2025
---
```
{
  "gm_0_0_0_aec_cp_ims_inventory_v1" : {
    "aliases" : {
      "gm_0_0_0_aec_cp_ims_inventory" : { }
    },
    "mappings" : {
      "properties" : {
        "asset" : {
          "properties" : {
            "dealerAssets" : {
              "properties" : {
                "assetType" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "category" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "dataSource" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "defaultAngle" : {
                  "type" : "boolean"
                },
                "display" : {
                  "type" : "boolean"
                },
                "provider" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
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
            "oemAssets" : {
              "properties" : {
                "angle" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "assetType" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "category" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "defaultAngle" : {
                  "type" : "boolean"
                },
                "provider" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "size" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
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
            "thirdPartyAssets" : {
              "properties" : {
                "assetType" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "category" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "dataSource" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "defaultAngle" : {
                  "type" : "boolean"
                },
                "provider" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            }
          }
        },
        "assets" : {
          "properties" : {
            "angle" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "assetType" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "category" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "dataSource" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "defaultAngle" : {
              "type" : "boolean"
            },
            "display" : {
              "type" : "boolean"
            },
            "provider" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "size" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "url" : {
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
        "availability" : {
          "properties" : {
            "activeDealersAvailable" : {
              "type" : "boolean"
            },
            "dcAvailabilityStatus" : {
              "properties" : {
                "name" : {
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
            "deleted" : {
              "type" : "boolean"
            },
            "inventoryStatus" : {
              "properties" : {
                "code" : {
                  "type" : "keyword"
                },
                "description" : {
                  "type" : "keyword"
                },
                "name" : {
                  "type" : "keyword"
                }
              }
            },
            "isAvailable" : {
              "type" : "boolean"
            },
            "purchaseStatus" : {
              "properties" : {
                "code" : {
                  "type" : "keyword"
                },
                "name" : {
                  "type" : "keyword"
                }
              }
            },
            "recallStatus" : {
              "type" : "keyword"
            },
            "tags" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "tekionInventoryStatus" : {
              "properties" : {
                "code" : {
                  "type" : "keyword",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword"
                    }
                  }
                },
                "description" : {
                  "type" : "keyword"
                },
                "name" : {
                  "type" : "keyword"
                }
              }
            }
          }
        },
        "color" : {
          "properties" : {
            "exteriorColor" : {
              "properties" : {
                "code" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "name" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
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
            "exteriorColorBase" : {
              "properties" : {
                "name" : {
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
            "interiorColor" : {
              "properties" : {
                "code" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "name" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
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
            "interiorColorBase" : {
              "properties" : {
                "name" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            }
          }
        },
        "dealerId" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "dealers" : {
          "properties" : {
            "dealers" : {
              "properties" : {
                "bac" : {
                  "type" : "keyword"
                },
                "businessManagementDivision" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "country" : {
                  "type" : "keyword"
                },
                "currencyCode" : {
                  "type" : "object",
                  "enabled" : false
                },
                "id" : {
                  "type" : "keyword"
                },
                "location" : {
                  "type" : "geo_point"
                },
                "name" : {
                  "type" : "object",
                  "enabled" : false
                },
                "postalCode" : {
                  "type" : "keyword"
                },
                "sellSource" : {
                  "properties" : {
                    "buick" : {
                      "type" : "boolean"
                    },
                    "cadillac" : {
                      "type" : "boolean"
                    },
                    "chevrolet" : {
                      "type" : "boolean"
                    },
                    "gmc" : {
                      "type" : "boolean"
                    }
                  }
                },
                "state" : {
                  "type" : "keyword"
                },
                "type" : {
                  "type" : "keyword"
                }
              }
            },
            "logisticCenter" : {
              "properties" : {
                "bac" : {
                  "type" : "keyword",
                  "fields" : {
                    "text" : {
                      "type" : "text"
                    }
                  }
                },
                "businessManagementDivision" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "country" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "location" : {
                  "properties" : {
                    "lat" : {
                      "type" : "float"
                    },
                    "lon" : {
                      "type" : "float"
                    }
                  }
                },
                "name" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "postalCode" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "sellSource" : {
                  "properties" : {
                    "buick" : {
                      "type" : "boolean"
                    },
                    "cadillac" : {
                      "type" : "boolean"
                    },
                    "chevrolet" : {
                      "type" : "boolean"
                    },
                    "gmc" : {
                      "type" : "boolean"
                    }
                  }
                },
                "state" : {
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
            "parentDealer" : {
              "properties" : {
                "bac" : {
                  "type" : "keyword"
                },
                "businessManagementDivision" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "country" : {
                  "type" : "keyword"
                },
                "currencyCode" : {
                  "type" : "object",
                  "enabled" : false
                },
                "id" : {
                  "type" : "keyword"
                },
                "location" : {
                  "type" : "geo_point"
                },
                "name" : {
                  "type" : "object",
                  "enabled" : false
                },
                "postalCode" : {
                  "type" : "keyword"
                },
                "sellSource" : {
                  "properties" : {
                    "buick" : {
                      "type" : "boolean"
                    },
                    "cadillac" : {
                      "type" : "boolean"
                    },
                    "chevrolet" : {
                      "type" : "boolean"
                    },
                    "gmc" : {
                      "type" : "boolean"
                    }
                  }
                },
                "state" : {
                  "type" : "keyword"
                },
                "type" : {
                  "type" : "keyword"
                }
              }
            }
          }
        },
        "id" : {
          "type" : "keyword",
          "fields" : {
            "text" : {
              "type" : "text"
            }
          }
        },
        "inventoryVersion" : {
          "type" : "long"
        },
        "locale" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "metaData" : {
          "properties" : {
            "deleted" : {
              "type" : "boolean"
            },
            "evenType" : {
              "type" : "keyword"
            },
            "eventSource" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "imsCreateTimestamp" : {
              "type" : "long"
            },
            "imsUpdateTimestamp" : {
              "type" : "long"
            },
            "integrationRawStorageKey" : {
              "type" : "keyword"
            },
            "integrationStorageKey" : {
              "type" : "keyword"
            },
            "messageId" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "oemId" : {
              "type" : "keyword"
            },
            "programId" : {
              "type" : "keyword"
            },
            "programIds" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "storageKey" : {
              "type" : "keyword"
            },
            "timeStamp" : {
              "type" : "long"
            },
            "updatedTimestamp" : {
              "type" : "long"
            },
            "version" : {
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
        "oemId" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "options" : {
          "properties" : {
            "category" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "cvdCode" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "description" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "disclosure" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "displayName" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "images" : {
              "properties" : {
                "size" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
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
            "oemCode" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "optionType" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "price" : {
              "type" : "float"
            }
          }
        },
        "packages" : {
          "properties" : {
            "category" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "description" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "disclosure" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "displayName" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "images" : {
              "properties" : {
                "size" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "url" : {
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
            "oemCode" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "optionType" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "price" : {
              "type" : "float"
            }
          }
        },
        "pricing" : {
          "properties" : {
            "cashPricing" : {
              "properties" : {
                "amount" : {
                  "type" : "float"
                },
                "name" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "type" : {
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
            "customerType" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "dealerPricing" : {
              "properties" : {
                "accessories" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "descriptions" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
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
                "addons" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "descriptions" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
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
                "basePrice" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "code" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "expirationDate" : {
                      "type" : "long"
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
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
                "fees" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "taxable" : {
                      "type" : "boolean"
                    },
                    "taxedSeparately" : {
                      "type" : "boolean"
                    },
                    "type" : {
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
                "totalPrice" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
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
                "upfits" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "descriptions" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    }
                  }
                }
              }
            },
            "finances" : {
              "properties" : {
                "apr" : {
                  "type" : "float"
                },
                "code" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "disclosure" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "expirationDate" : {
                  "type" : "long"
                },
                "financialProvider" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "financialProviderDescription" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "netPrice" : {
                  "type" : "float"
                },
                "netPriceWithDealerFees" : {
                  "type" : "float"
                },
                "provider" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "term" : {
                  "type" : "long"
                },
                "type" : {
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
            "incentives" : {
              "properties" : {
                "cash" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "code" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "descriptions" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "expirationDate" : {
                      "type" : "long"
                    },
                    "isLocked" : {
                      "type" : "boolean"
                    },
                    "mathBoxType" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "originalType" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "provider" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
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
                "finance" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "code" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "descriptions" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "expirationDate" : {
                      "type" : "long"
                    },
                    "isLocked" : {
                      "type" : "boolean"
                    },
                    "mathBoxType" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "originalType" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "provider" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "term" : {
                      "type" : "long"
                    },
                    "type" : {
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
                "lease" : {
                  "properties" : {
                    "amount" : {
                      "type" : "float"
                    },
                    "code" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "descriptions" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "disclosure" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "expirationDate" : {
                      "type" : "long"
                    },
                    "isLocked" : {
                      "type" : "boolean"
                    },
                    "mathBoxType" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "name" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "originalType" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "provider" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "term" : {
                      "type" : "long"
                    },
                    "type" : {
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
                "total" : {
                  "properties" : {
                    "totalCashIncentives" : {
                      "properties" : {
                        "amount" : {
                          "type" : "float"
                        },
                        "mathBoxType" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "name" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "provider" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "type" : {
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
                    "totalFinanceIncentives" : {
                      "properties" : {
                        "amount" : {
                          "type" : "float"
                        },
                        "mathBoxType" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "name" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "provider" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "term" : {
                          "type" : "long"
                        },
                        "type" : {
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
                    "totalLeaseIncentives" : {
                      "properties" : {
                        "amount" : {
                          "type" : "float"
                        },
                        "mathBoxType" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "name" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "provider" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "term" : {
                          "type" : "long"
                        },
                        "type" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            },
            "lease" : {
              "properties" : {
                "apr" : {
                  "type" : "float"
                },
                "disclosure" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "expirationDate" : {
                  "type" : "long"
                },
                "netPrice" : {
                  "type" : "float"
                },
                "netPriceWithDealerFees" : {
                  "type" : "float"
                },
                "provider" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "residual" : {
                  "properties" : {
                    "10000" : {
                      "properties" : {
                        "mileage" : {
                          "type" : "long"
                        },
                        "residualRate" : {
                          "type" : "float"
                        }
                      }
                    },
                    "12000" : {
                      "properties" : {
                        "mileage" : {
                          "type" : "long"
                        },
                        "residualRate" : {
                          "type" : "float"
                        }
                      }
                    },
                    "15000" : {
                      "properties" : {
                        "mileage" : {
                          "type" : "long"
                        },
                        "residualRate" : {
                          "type" : "float"
                        }
                      }
                    },
                    "9230" : {
                      "properties" : {
                        "mileage" : {
                          "type" : "long"
                        }
                      }
                    }
                  }
                },
                "term" : {
                  "type" : "long"
                },
                "type" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "waiveFirstMonthPayment" : {
                  "type" : "boolean"
                },
                "waiveSecurityDeposit" : {
                  "type" : "boolean"
                }
              }
            },
            "oemPricing" : {
              "properties" : {
                "amount" : {
                  "type" : "float"
                },
                "childOptionCodes" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "code" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "consumerFriendlyDescriptions" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "descriptions" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "name" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "type" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            }
          }
        },
        "programId" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "recall" : {
          "properties" : {
            "actionNumber" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "actionType" : {
              "type" : "keyword"
            },
            "actionTypeDescription" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "buCode" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "fieldActnTitle" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "langCode" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "notes" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "partAvailability" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "safetyRisk" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "statusCode" : {
              "type" : "keyword"
            },
            "vin" : {
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
        "stock" : {
          "properties" : {
            "condition" : {
              "type" : "keyword"
            },
            "estimatedDeliveryDate" : {
              "type" : "long"
            },
            "inStockDate" : {
              "type" : "long"
            },
            "productionDate" : {
              "type" : "long"
            },
            "stockNumber" : {
              "type" : "keyword"
            },
            "type" : {
              "type" : "keyword"
            }
          }
        },
        "techSpec" : {
          "properties" : {
            "capacity" : {
              "properties" : {
                "noOfDoors" : {
                  "type" : "long"
                },
                "payload" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "long"
                    }
                  }
                },
                "seating" : {
                  "properties" : {
                    "capacity" : {
                      "type" : "long"
                    },
                    "rows" : {
                      "type" : "long"
                    }
                  }
                },
                "trailering" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "long"
                    }
                  }
                }
              }
            },
            "driveType" : {
              "properties" : {
                "code" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "description" : {
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
            "engine" : {
              "properties" : {
                "code" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "cylinder" : {
                  "properties" : {
                    "count" : {
                      "type" : "long"
                    }
                  }
                },
                "desc" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "displacement" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "float"
                    }
                  }
                },
                "fuelSystem" : {
                  "properties" : {
                    "description" : {
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
                "horsePower" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "long"
                    }
                  }
                }
              }
            },
            "fuel" : {
              "properties" : {
                "capacity" : {
                  "properties" : {
                    "tank" : {
                      "properties" : {
                        "unit" : {
                          "type" : "text",
                          "fields" : {
                            "keyword" : {
                              "type" : "keyword",
                              "ignore_above" : 256
                            }
                          }
                        },
                        "value" : {
                          "type" : "float"
                        }
                      }
                    }
                  }
                },
                "fuelType" : {
                  "properties" : {
                    "description" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "type" : {
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
                "performance" : {
                  "properties" : {
                    "batteryRange" : {
                      "properties" : {
                        "max" : {
                          "type" : "long"
                        },
                        "min" : {
                          "type" : "long"
                        },
                        "unit" : {
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
                    "economy" : {
                      "properties" : {
                        "city" : {
                          "properties" : {
                            "high" : {
                              "type" : "long"
                            },
                            "low" : {
                              "type" : "long"
                            },
                            "unit" : {
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
                        "combined" : {
                          "properties" : {
                            "high" : {
                              "type" : "long"
                            },
                            "low" : {
                              "type" : "long"
                            },
                            "unit" : {
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
                        "highway" : {
                          "properties" : {
                            "high" : {
                              "type" : "long"
                            },
                            "low" : {
                              "type" : "long"
                            },
                            "unit" : {
                              "type" : "text",
                              "fields" : {
                                "keyword" : {
                                  "type" : "keyword",
                                  "ignore_above" : 256
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            },
            "rearWheel" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "suspension" : {
              "properties" : {
                "type" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "value" : {
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
            "transmission" : {
              "properties" : {
                "gears" : {
                  "properties" : {
                    "shifter" : {
                      "type" : "object"
                    },
                    "speed" : {
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
                "type" : {
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
            "weightsPayloadTowing" : {
              "properties" : {
                "asSpecdPayload" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "float"
                    }
                  }
                },
                "deadWeightHitchMax" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "float"
                    }
                  }
                },
                "payloadWeightFront" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "float"
                    }
                  }
                },
                "payloadWeightRear" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "float"
                    }
                  }
                },
                "weightDistributingHitchMax" : {
                  "properties" : {
                    "unit" : {
                      "type" : "text",
                      "fields" : {
                        "keyword" : {
                          "type" : "keyword",
                          "ignore_above" : 256
                        }
                      }
                    },
                    "value" : {
                      "type" : "float"
                    }
                  }
                }
              }
            },
            "wheel" : {
              "properties" : {
                "desc" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "oemCode" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            }
          }
        },
        "tenantId" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "vehicleBasic" : {
          "properties" : {
            "category" : {
              "properties" : {
                "bodyStyle" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "boxType" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "series" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "type" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "vehicleType" : {
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
            "doors" : {
              "type" : "long"
            },
            "make" : {
              "type" : "keyword"
            },
            "model" : {
              "type" : "keyword"
            },
            "odometer" : {
              "properties" : {
                "mileage" : {
                  "type" : "float"
                }
              }
            },
            "seatingCapacity" : {
              "type" : "long"
            },
            "variant" : {
              "properties" : {
                "chromeStyleId" : {
                  "type" : "keyword"
                },
                "merchandisingModelCode" : {
                  "type" : "keyword"
                },
                "name" : {
                  "type" : "keyword"
                },
                "trimCode" : {
                  "type" : "keyword"
                }
              }
            },
            "year" : {
              "type" : "short"
            }
          }
        },
        "vehicleIdentity" : {
          "properties" : {
            "id" : {
              "type" : "keyword"
            },
            "vin" : {
              "type" : "keyword"
            }
          }
        },
        "vin" : {
          "type" : "keyword"
        },
        "warranty" : {
          "properties" : {
            "description" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "expirationDate" : {
              "type" : "long"
            },
            "label" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "mileage" : {
              "properties" : {
                "endMileage" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "startMileage" : {
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
            "note" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "startDate" : {
              "type" : "long"
            },
            "status" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "terms" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "mapping" : {
          "total_fields" : {
            "limit" : "3000"
          }
        },
        "number_of_shards" : "2",
        "provided_name" : "gm_0_0_0_aec_cp_ims_inventory_v1",
        "creation_date" : "1714646682018",
        "number_of_replicas" : "2",
        "uuid" : "zP5vxR_QQXmd5sfPxcoW7A",
        "version" : {
          "created" : "7170399"
        }
      }
    }
  }
}

```