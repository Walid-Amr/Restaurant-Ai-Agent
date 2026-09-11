<img width="1491" height="522" alt="Screenshot 2026-09-02 122320" src="https://github.com/user-attachments/assets/2a42525e-5123-4026-b149-b882d4547667" />
[My workflow (2).json](https://github.com/user-attachments/files/32131553/My.workflow.2.json)
{
  "name": "My workflow",
  "nodes": [
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.5,
      "position": [
        -48,
        -48
      ],
      "id": "237e7fad-c455-4908-be5a-0b6133b9b4eb",
      "name": "Telegram Trigger",
      "webhookId": "d1db9a36-8606-4d80-a5ac-fce11fc22cf6",
      "credentials": {
        "telegramApi": {
          "id": "uFh2CV3YcRbqwJ97",
          "name": "Telegram account 2"
        }
      }
    },
    {
      "parameters": {
        "model": "inclusionai/ling-3.0-flash-fin:free",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenRouter",
      "typeVersion": 1,
      "position": [
        96,
        192
      ],
      "id": "ab0bbf4f-5b72-456e-9d41-a91680267180",
      "name": "OpenRouter Chat Model",
      "credentials": {
        "openRouterApi": {
          "id": "uPn7ftBdypmB8NVX",
          "name": "OpenRouter account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.from.id }}",
        "text": "={{ $json.output.message }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        512,
        -48
      ],
      "id": "a991dfc8-fdb2-48db-b422-84be58e12c77",
      "name": "Send a text message",
      "webhookId": "5837f6ed-0271-4d1f-b6d8-764a00a5d2e3",
      "credentials": {
        "telegramApi": {
          "id": "uFh2CV3YcRbqwJ97",
          "name": "Telegram account 2"
        }
      }
    },
    {
      "parameters": {
        "schemaType": "manual",
        "inputSchema": "{\n  \"type\": \"object\",\n  \"properties\": {\n    \"message\": {\n      \"type\": \"string\",\n      \"description\": \"LLM Reply\"\n    },\n    \"is_chosen\": {\n      \"type\": \"boolean\",\n      \"description\": \"True only if the customer confirmed a sandwich AND provided both their name and phone number\"\n    },\n    \"name\": {\n      \"type\": \"string\",\n      \"description\": \"Sandwich Name if a sandwich was chosen\"\n    },\n    \"price\": {\n      \"type\": \"string\",\n      \"description\": \"Sandwich Price if a sandwich was chosen\"\n    },\n    \"ingredients\": {\n      \"type\": \"string\",\n      \"description\": \"Sandwich Ingredients if a sandwich was chosen\"\n    },\n    \"customer_name\": {\n      \"type\": \"string\",\n      \"description\": \"Customer's name, once provided\"\n    },\n    \"customer_phone\": {\n      \"type\": \"string\",\n      \"description\": \"Customer's phone number, once provided\"\n    }\n  }\n}",
        "autoFix": true,
        "customizeRetryPrompt": true
      },
      "type": "@n8n/n8n-nodes-langchain.outputParserStructured",
      "typeVersion": 1.3,
      "position": [
        368,
        176
      ],
      "id": "adf9be95-2796-4a17-96ee-d2ef8ba2febc",
      "name": "Structured Output Parser"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "cachedResultName": "Salta3 Orders",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1P6FqASXlxmURilQQWLCEJnBD83aY6qisIxb06xHhKxI/edit?usp=drivesdk",
          "mode": "list",
          "value": "1P6FqASXlxmURilQQWLCEJnBD83aY6qisIxb06xHhKxI"
        },
        "sheetName": {
          "__rl": true,
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1P6FqASXlxmURilQQWLCEJnBD83aY6qisIxb06xHhKxI/edit#gid=0",
          "mode": "list",
          "value": "gid=0"
        },
        "columns": {
          "attemptToConvertTypes": false,
          "convertFieldsToString": false,
          "mappingMode": "defineBelow",
          "matchingColumns": [],
          "schema": [
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "Date",
              "id": "Date",
              "removed": false,
              "required": false,
              "type": "string"
            },
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "Item",
              "id": "Item",
              "removed": false,
              "required": false,
              "type": "string"
            },
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "For",
              "id": "For",
              "removed": false,
              "required": false,
              "type": "string"
            },
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "Phone ",
              "id": "Phone ",
              "removed": false,
              "required": false,
              "type": "string"
            },
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "Status",
              "id": "Status",
              "removed": false,
              "required": false,
              "type": "string"
            },
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "Order Number",
              "id": "Order Number",
              "removed": false,
              "required": false,
              "type": "string"
            }
          ],
          "value": {
            "Date": "={{ $now.toISODate() }}",
            "For": "={{ $('AI Agent').item.json.output.customer_name }}",
            "Item": "={{ $('AI Agent').item.json.output.name }}",
            "Order Number": "={{ $('Edit Fields').item.json['Last Number'] }}",
            "Phone ": "={{ $('AI Agent').item.json.output.customer_phone }}",
            "Status": "In kitchen"
          }
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1408,
        -160
      ],
      "id": "da912332-8ab3-42ac-ae62-38d5d7aca5b2",
      "name": "Append row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "wzhsIh3ok101yrkr",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $('AI Agent').item.json.output.is_chosen }}",
                    "rightValue": false,
                    "operator": {
                      "type": "boolean",
                      "operation": "true",
                      "singleValue": true
                    },
                    "id": "b34801d1-3ff8-406a-8a86-dc3bfbfc9ec4"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "True"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "bd56b564-024f-4a54-8039-613efd337faa",
                    "leftValue": "={{ $('AI Agent').item.json.output.is_chosen }}",
                    "rightValue": false,
                    "operator": {
                      "type": "boolean",
                      "operation": "false",
                      "singleValue": true
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "False"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        768,
        -48
      ],
      "id": "dd937cec-a100-4700-9e02-37c53ffc9983",
      "name": "Switch"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.message.text }}",
        "hasOutputParser": true,
        "options": {
          "systemMessage": "You are my restaurant orders assistant. You get inquiries about our menu. Help users to choose the best suitable sandwich.\n\n### Menu\n\nName: Burger with Cheese\nPrice: 10 EGP\nIngredients: [Burger, Bread, Cheese]\n\nName: Burger\nPrice: 5 EGP\nIngredients: [Burger, Bread]\n\nName: Cheese\nPrice: 2 EGP\nIngredients: [Cheese, Bread]\n\nInstructions:\n** Be kind\n** Do not use emojis in your responses.\n** Always reply in the same language the customer used in their message.\n** Keep your tone warm and natural but concise — avoid excessive enthusiasm or exclamation marks.\n** Make sure the output is marked up and easy to read\n** If you would suggest a sandwich, give it in this style:\n*** Name:\n*** Price:\n** When the customer confirms an order, format the confirmation the same way (Name, Price), then ask if they would like to add anything else.\n** Only set is_chosen to true if the customer explicitly confirms or orders a specific sandwich by name. A suggestion or recommendation from you does NOT count as chosen — is_chosen must stay false until the customer clearly agrees, confirms, or asks for it.\n** If the customer hasn't specified a preference yet, ask a clarifying question instead of assuming a choice for them.\n** Respond with raw JSON only that matches the exact schema. Do not wrap the JSON in markdown code blocks. Do not nest it under any extra key like \"output\" — the fields must be at the top level\n** After the customer confirms a specific sandwich, before finalizing the order, ask for their name and phone number if you don't have them yet in this conversation.\n** Only set is_chosen to true once the customer has confirmed a sandwich AND you have both their name and phone number. Until then, keep is_chosen false even if a sandwich was already confirmed.\n** Once you have the sandwich, name, and phone number, confirm the full order back to the customer before finalizing\n** ORDER FINALIZATION & RESET:\n- Set \"is_chosen\" to true ONLY AT THE EXACT MOMENT the customer confirms a sandwich AND provides both their name and phone number.\n- Once you output the final order summary (with sandwich, price, name, and phone), that order is FULLY COMPLETED.\n- For ANY message sent by the customer AFTER a completed order (such as greetings like \"Hi\", \"السلام عليكم\", or asking for new sandwiches): reset your state, keep \"is_chosen\" as false, greet them naturally, and treat it as a brand NEW inquiry or order.\n- NEVER repeat an already completed order summary when the customer starts a new message or greeting."
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        208,
        -48
      ],
      "id": "d3edde34-e34b-46ff-a4b4-1ec78bed6af4",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $json.message.chat.id }}"
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        224,
        192
      ],
      "id": "447422d7-eb9f-4dfb-a67f-77be2958aaeb",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "model": "dots-studio/dots-3-note-preview:free",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenRouter",
      "typeVersion": 1,
      "position": [
        464,
        304
      ],
      "id": "f0f24a11-d622-4b92-a451-0cf7c3e4e4e8",
      "name": "OpenRouter Chat Model1",
      "credentials": {
        "openRouterApi": {
          "id": "uPn7ftBdypmB8NVX",
          "name": "OpenRouter account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "cachedResultName": "Salta3 Orders",
          "mode": "list",
          "value": "1P6FqASXlxmURilQQWLCEJnBD83aY6qisIxb06xHhKxI"
        },
        "sheetName": {
          "__rl": true,
          "mode": "name",
          "value": "Sheet2"
        },
        "options": {}
      },
      "id": "b6b00e02-d2fd-4bb7-8bff-60f42d55381f",
      "name": "Get row(s) in sheet2",
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        960,
        -160
      ],
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "wzhsIh3ok101yrkr",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "a1b2c3d4-e5f6-4789-a012-3456789abcde",
              "name": "Last Number",
              "type": "number",
              "value": "={{ $json['Last Number'] + 1 }}"
            }
          ]
        },
        "options": {}
      },
      "id": "aa0c35c0-8589-49f0-9c28-0df73aa8c8d0",
      "name": "Edit Fields",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        1184,
        -160
      ]
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "cachedResultName": "Salta3 Orders",
          "mode": "list",
          "value": "1P6FqASXlxmURilQQWLCEJnBD83aY6qisIxb06xHhKxI"
        },
        "sheetName": {
          "__rl": true,
          "mode": "name",
          "value": "Sheet2"
        },
        "columns": {
          "attemptToConvertTypes": false,
          "convertFieldsToString": false,
          "mappingMode": "defineBelow",
          "matchingColumns": [
            "row_number"
          ],
          "schema": [
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "row_number",
              "id": "row_number",
              "removed": false,
              "required": false,
              "type": "string"
            },
            {
              "canBeUsedToMatch": true,
              "defaultMatch": false,
              "display": true,
              "displayName": "Last Number",
              "id": "Last Number",
              "removed": false,
              "required": false,
              "type": "string"
            }
          ],
          "value": {
            "Last Number": "={{ $('Edit Fields').item.json['Last Number'] }}",
            "row_number": "={{ $('Get row(s) in sheet2').item.json.row_number }}"
          }
        },
        "options": {}
      },
      "id": "2b275b29-ff2c-4c54-a0cf-f034c7576f25",
      "name": "Update row in sheet2",
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1632,
        -160
      ],
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "wzhsIh3ok101yrkr",
          "name": "Google Sheets account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenRouter Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Structured Output Parser": {
      "ai_outputParser": [
        [
          {
            "node": "AI Agent",
            "type": "ai_outputParser",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet": {
      "main": [
        [
          {
            "node": "Update row in sheet2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Send a text message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "Send a text message": {
      "main": [
        [
          {
            "node": "Switch",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenRouter Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "Structured Output Parser",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Switch": {
      "main": [
        [
          {
            "node": "Get row(s) in sheet2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get row(s) in sheet2": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "Append row in sheet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "availableInMCP": true
  },
  "versionId": "ced65f6d-746f-482f-8398-90ec4851c805",
  "meta": {
    "templateCredsSetupCompleted": true,
    "aiBuilderAssisted": true,
    "builderVariant": "mcp",
    "instanceId": "3a1c6fde203b1f5f5197697ac31bcd7cc700cc46a52c5f634533582175a3c80a"
  },
  "nodeGroups": [],
  "id": "oTx7NBsRPjCvELAY",
  "tags": []
}

# Restaurant-Ai-Agent
Building an AI-Powered Restaurant Ordering Assistant with n8n — A Case Study
The Problem

Small restaurants often rely on manual order-taking: a customer calls or messages, someone jots down the order, walks it to the kitchen, and later updates the inventory by hand. Every step is a chance for a mistake — a missed order, a stock count that's never updated, a customer who has to repeat their order because nobody wrote down their name or phone number correctly.

I wanted to see how far I could take this with pure automation: a customer messages a Telegram bot, the bot understands what they want in natural language, confirms the order, logs it, and keeps inventory in sync — with zero manual data entry.

The Solution

I built a multi-stage automation system in n8n, connected to Google Sheets and Telegram, with an AI agent at its core. The final piece — and the focus of this report — is the ordering assistant itself: a conversational AI that talks to customers, understands their order in plain language (in whatever language they write in), collects what's needed to complete it, and writes it straight into the restaurant's order log.

Technical Architecture

1. Telegram Trigger The flow starts the moment a customer sends a message to the restaurant's bot.

2. AI Agent This is the core of the system. It's given a system prompt containing the full menu (items, prices, ingredients) and a set of behavioral rules: reply in the customer's language, never assume a choice the customer hasn't confirmed, ask clarifying questions instead of guessing, and only mark an order as "chosen" once both a specific item and the customer's name and phone number have been provided.

The Agent is wired to three sub-components:

Model — an OpenRouter-connected chat model that powers the actual reasoning and replies.
Memory — a Window Buffer Memory node, keyed to each customer's Telegram chat ID, so the Agent remembers earlier messages in the same conversation (what they ordered, whether they've given their name yet, etc.).
Output Parser — a Structured Output Parser that forces every response into a strict JSON schema (message, is_chosen, name, price, ingredients, customer_name, customer_phone), with its own connected model to auto-fix malformed output when needed.

3. Send a text message The Agent's reply (message) is sent straight back to the customer on Telegram — whether that's a clarifying question, a suggestion, or an order confirmation.

4. Switch Branches the flow based on is_chosen. If the order isn't finalized yet (still gathering details), the flow stops here — the customer just gets the reply. If the order is confirmed, it moves on to be recorded.

5. Get row(s) in sheet2 → Edit Fields → Append row in sheet → Update row in sheet2 This is the order-logging and counter chain: the current order number is read from a counter sheet, incremented by one, used to log the new order (item, customer name, phone, date, status) into the main orders sheet, and the incremented counter is written back — so every order gets a unique, sequential number.

Challenges & Fixes

Building this wasn't a straight line. A few of the more interesting problems along the way:

Silent duplicate order numbers. Two different orders ended up with the same order number. Tracing it back, the "increment the counter" step was entirely missing from this branch — every order was reading the same stale number instead of incrementing it.
A hardcoded status bug. While fixing the above, I found every new order was being logged with status "Done" instead of "In kitchen" — meaning orders looked complete the instant they were placed, before the kitchen had even seen them.
Case-sensitive spreadsheet headers. Google Sheets column matching in n8n is case-sensitive; a header typed as "Last number" instead of "Last Number" silently broke every downstream reference to it.
The AI "forgetting" the conversation. Early on, the bot would re-greet a customer who had already chosen a sandwich, instead of continuing the order. The fix required moving from a stateless LLM chain to an AI Agent node with proper session-based memory, keyed to each customer's chat ID.
Malformed AI output. The model would occasionally wrap its JSON reply in markdown formatting or nest it under an unexpected key, breaking the structured parser. This needed both explicit formatting instructions in the prompt and a fallback auto-fix step.
Rate limits on free AI models. Heavy testing against a free-tier model hit its daily rate limit quickly, which meant pacing test runs and understanding exactly how usage, credits, and rate limits work on the model provider's platform.
Result

A customer can now message the bot in Arabic, English, or French, get a natural reply, confirm a sandwich, provide their name and number conversationally over a few messages, and have the order logged automatically with a sequential order number and correct kitchen status — no manual spreadsheet work at any point.

Lessons Learned

Most of the hardest bugs here weren't in the AI logic — they were in the plumbing around it: a missing connection, a hardcoded value left over from an earlier version, a column name that didn't match exactly. Debugging an AI-driven workflow means checking the deterministic parts (the sheet, the counters, the matching columns) just as carefully as the AI's behavior itself.

Built with n8n, Google Sheets, and Telegram, with Claude (Anthropic) used throughout as an AI pair-programmer — for debugging each stage, refining the prompt logic, and, toward the end, connecting directly into the n8n workflow via its MCP integration to inspect and fix the configuration directly.
