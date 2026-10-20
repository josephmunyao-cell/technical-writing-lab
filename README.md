# Technical Writing Lab: Procedure, API Reference, and Report Section
**Student Name:** JOSEPH MUNYAO KIIO  
**Course:** Power Learn Project (PLP) - Software Engineering  

---

## Exercise A: User Manual Procedure

**Task Documented:** Setting up a GitHub repository from scratch and making a first commit.  
**Audience:** First-semester computing student with basic computer literacy but no prior experience with Git or GitHub.

### Prerequisites
- A computer running Windows, macOS, or Linux with administrator access.
- Git installed on your computer (check by running `git --version` in terminal).
- A free GitHub account created at [github.com](https://github.com).
- A text editor or IDE (such as Visual Studio Code).
- Basic familiarity with terminal commands.
- An active internet connection.

### Procedure
1. Sign in to your GitHub account in a web browser.  
   *Expected result:* The GitHub dashboard loads, showing your profile icon in the top-right corner.
2. Click the **+** icon in the top-right corner and select **"New repository"**.  
   *Expected result:* The "Create a new repository" page opens.
3. Enter a repository name (e.g., `my-first-repo`) in the **Repository name** field.  
   *Expected result:* GitHub shows a green checkmark next to the name if available.
4. Select **Public** or **Private**, then check **"Add a README file"**.  
   *Expected result:* The option is marked as selected with a filled checkbox.
5. Click the green **"Create repository"** button at the bottom.  
   *Expected result:* GitHub creates the repository and redirects you to its main page displaying `README.md`.
6. Click the green **"Code"** button and copy the HTTPS URL shown.  
   *Expected result:* The URL ending in `.git` is copied to your clipboard.
7. Open terminal/Command Prompt and navigate to your target directory using `cd`.  
   *Expected result:* The terminal prompt reflects the target directory path.
8. Run `git clone <copied-url>`.  
   *Expected result:* The terminal displays `Cloning into 'my-first-repo'...` and downloads the repository folder.
9. Move into the repository folder using `cd my-first-repo`.  
   *Expected result:* The terminal prompt displays the repository folder name.
10. Create a file named `notes.txt` inside the folder with a line of text.  
    *Expected result:* The file is visible when running `dir` (Windows) or `ls` (macOS/Linux).
11. Stage the new file by running `git add notes.txt`.  
    *Expected result:* The command completes quietly with no output, indicating success.
12. Commit the staged file by running `git commit -m "Add notes file"`.  
    *Expected result:* Terminal outputs a summary line like `1 file changed, 1 insertion(+)`.
13. Upload your changes by running `git push origin main`.  
    *Expected result:* Terminal shows progress and concludes with `main -> main`.
14. Refresh the repository page in your browser.  
    *Expected result:* `notes.txt` appears in the file list with your commit message beside it.

#### Screenshot Description
A terminal window screenshot displaying successful output following `git push origin main`, ending with `main -> main`.

### Troubleshooting
> ⚠ **Common Error:** `fatal: Authentication failed` when running `git push`  
> **Cause & Solution:** GitHub no longer accepts account passwords for command-line authentication. Generate a Personal Access Token (PAT) under **Settings > Developer settings > Personal access tokens**, and use the token in place of your password when prompted.

---

## Exercise B: API Reference Entry

### Endpoint
`POST /api/v1/projects/{projectId}/tasks`

### Description
Creates a new task inside the specified project. The requesting user must be authenticated and a member of the target project.

### Path Parameters
| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `projectId` | string (UUID) | Required | Unique identifier of the target project. |

### Query Parameters
*None.*

### Request Body Parameters
| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `title` | string | Required | Task title (1–200 characters). |
| `description` | string | Optional | Detailed plain-text or Markdown description. |
| `assigneeId` | string (UUID) | Optional | User ID of assigned member. |
| `dueDate` | string (ISO 8601) | Required | Completion target date formatted as `YYYY-MM-DD`. |
| `priority` | string (enum) | Required | Priority level: `low`, `medium`, or `high`. |

### Request Headers
| Header | Required | Description |
| :--- | :--- | :--- |
| `Authorization` | Required | Bearer token formatted as `Bearer <access_token>`. |
| `Content-Type` | Required | Must be `application/json`. |

### Response Codes
| Code | Meaning | Description |
| :--- | :--- | :--- |
| **201** | Created | Task successfully created. Returns task object. |
| **400** | Bad Request | Missing required fields or invalid input payload. |
| **401** | Unauthorized | Missing, invalid, or expired authentication token. |
| **403** | Forbidden | Authenticated user is not a member of the project. |
| **404** | Not Found | Project or assignee user ID does not exist. |
| **500** | Internal Server Error | Server failed to process request. |

### Example Request
```http
POST /api/v1/projects/8f14e45f-ceea-4c5a-b8b3-9d2f1a0e4c21/tasks HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json

{
  "title": "Prepare Q3 budget review slides",
  "description": "Summarize actuals vs. forecast for the finance meeting.",
  "assigneeId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "dueDate": "2026-09-15",
  "priority": "high"
}