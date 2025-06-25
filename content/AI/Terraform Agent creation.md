
```
module "account-summariser" {
  source = "gitlab.com/demandbase/agent-without-lambda/aws"

  agent_name              = "account-summariser"
  foundational_model_name = "anthropic.claude-3-5-sonnet-20240620-v1:0"
  db_purpose              = "sales-ai-service"
  db_project              = "sales-ai-service"
  db_cloud                = "DB1S"
  db_owner                = "ivt"
  db_subpurpose           = "sales-ai-service"
  db_environment          = "development"
  aws_account_id          = var.aws_account_id
  instruction             = "You are a summarization expert with a strong ability to write clear and concise technical summaries."
  default_orchestration_prompt_override_configuration = [
    {
      base_prompt_template = "{\"anthropic_version\":\"bedrock-2023-05-31\",\"system\":\"\",\"messages\":[{\"role\":\"user\",\"content\":\"\"}]}"
      inference_configuration = [
        {
          temperature    = 0.4
          top_p          = 0.9
          max_length     = 800
          stop_sequences = ["\n\nHuman:"]
          top_k          = 250
        }
      ]
      parser_mode          = "DEFAULT"
      prompt_creation_mode = "OVERRIDDEN"
      prompt_state         = "ENABLED"
      prompt_type          = "ORCHESTRATION"
    }
  ]
}

```

