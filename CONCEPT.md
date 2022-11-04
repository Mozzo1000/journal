# Concept
This document contains ideas, brainstorming and how I think `journal` will work both tecnically but also for the end user before a implementation is made.

## API endpoints


### Get all journals
Returns all journals stored on server.
| URL            | Method | Authentication |
| -------------- | ------ | -------------- |
| `/v1/journals` | `GET`  | Required       |

<details>
<summary><strong>Header</strong></summary>
<code>Authorization: Bearer xxx</code>
</details>

<details>
<summary><strong>Response</strong></summary>

| HTTP Code | Content-Type       | Response                                                 |
| --------- | ------------------ | -------------------------------------------------------- |
| 200       | `application/json` | `{}`                                                     |
| 401       | `application/json` | `{"message": "Invalid or missing authentication token"}` |
| 404       | `application/json` | `{"message": "No data stored on the server"}`            |

</details>

### Add journal
Add a new journal to the server.
| URL            | Method | Authentication |
| -------------- | ------ | -------------- |
| `/v1/journals` | `POST` | Required       |

<details>
<summary><strong>Header</strong></summary>
<code>Authorization: Bearer xxx</code>

<code>Form Data...</code>
</details>

<details>
<summary><strong>Response</strong></summary>

| HTTP Code | Content-Type       | Response                                                 |
| --------- | ------------------ | -------------------------------------------------------- |
| 200       | `application/json` | `{"message": "New journal added successfully}`           |
| 400       | `application/json` | `{"message": "Bad request. Journal may already exist"}`  |
| 401       | `application/json` | `{"message": "Invalid or missing authentication token"}` |

</details>


## Database schema

**Table: documents**
| Name         | Type     | Nullable |
| ------------ | -------- | -------- |
| id           | int      | False    |
| filename     | varchar  | False    |
| hash         | varshar  | False    |
| created_at   | datetime | False    |
| last_updated | datetime | False    |

## CLI tool

* `journal init` 
  * Creates a hidden database file in the directory specified. The file should be named `.journal.db`. 
  * The directory should be empty but it is not required as the tool should not do any damage on existing files.
  * If the directory is not empty, warn the user. 

* `journal status`
  * Can only be run in a folder in which there is a `.journal.db` file inside.
  * Will look inside the folder for any markdown files and compare there hashes to the once in the `.journal.db` file. 
    * If a name of a file inside the folder does not exist in the database, then show the file as "NEW".
    * If a filename and hash both exists in the database, then do nothing.
    * If a filename exists in the database but the hash is not the same, show the file as "MODIFIED".

* `journal sync`
  * Downloads any file which does not exist in the local database
  * Uploads any file which does not exist in the remote database or which has a different hash then in the local.