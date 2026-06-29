# 🤖 100+ file solution (n8n AI Workflow)

An enterprise-grade, automated backend workflow built in n8n designed to handle bulk document processing (**100+ files at a time**). The system monitors a Google Drive folder, dynamically extracts text from massive incoming batches of documents (PDFs & Word Docs), generates intelligent summaries using the **Google Gemini AI Model**, and logs the structured data safely into Google Sheets without breaking rate limits.

---

## 📊 Workflow Visual Canvas

Below is the live structure of the automation pipeline:

<img width="1865" height="808" alt="Screenshot 2026-06-29 222216" src="https://github.com/user-attachments/assets/4ff50ba3-e414-42ff-9b59-46077a09b5ed" />


---

## 🚀 Key Features

* **Bulk File Management**: Specifically architected as a **100+ file solution** to systematically discover and handle high volumes of files from Google Drive.
* **Smart File Routing**: Uses conditional switch logic to route files based on extensions (`.pdf` / `.doc`).
* **Advanced Text Extraction**: Seamlessly extracts raw text content from both PDF and Word formats.
* **AI-Powered Summarization**: Leverages the **Google Gemini LLM** (via Advanced AI nodes) to extract key insights and generate summaries.
* **Batch Processing & Rate Limiting (Looping)**: Implements sequential data processing (one-by-one) via a control loop. This is critical for the *100+ file solution* to optimize system memory and prevent API rate-limit blocks from Google and Gemini.
* **Structured Data Logging**: Appends processed filenames along with their AI summaries into a designated Google Sheet.

---

## 📐 Workflow Architecture

The sequential flow of data across the canvas operates as follows:

```text
[Execute Workflow] ➔ [Search Files/Folders] ➔ [Process One-by-One (Loop)]
                                                    │
             ┌──────────────────────────────────────┘
             ▼
      [Download File]
             │
     [Check Extension] ─── (PDF) ───➔ [Extract From PDF] ───┐
             │                                              ▼
          (DOC) ────────────────────➔ [Extract From DOC] ➔ [Merge Text]
                                                            │
                                                            ▼
                                                   [Prepare Sheet Data]
                                                            │
                                                            ▼
                                                   [Summarize (Gemini)]
                                                            │
                                                            ▼
                                                  [Append to Google Sheet]
```

### Detailed Node Breakdown

1. **Trigger (`Execute Workflow`)**: Manually starts the execution sequence on demand for processing the file batch.
2. **Search Files and Folders**: References the specified target directory inside Google Drive to list all queued files (capable of fetching 100+ entities).
3. **Process Files One by One**: Acts as a control loop iterator to process one file entity at a time, ensuring safety from system crashes during bulk runs.
4. **Download File**: Downloads the stream of the active loop binary object locally into the execution container.
5. **Check File Extension**: Evaluates the routing rules (`.pdf` vs `.doc`) to assign the correct downstream text conversion algorithm.
6. **Extract from PDF / DOC**: Converts binary documents into readable text payloads.
7. **Merge Extracted Text**: Unifies the variable data paths into a structured JSON string format.
8. **Summarize Text (`Google Gemini Model`)**: Feeds the consolidated text chunk into the Gemini AI engine with custom contextual memory tools to generate clean summaries.
9. **Prepare Sheet Data & Append**: Normalizes the final text output and cleanly pushes a new structured entry row containing `Filename` and `Summary` into the target Google Sheet.

---

## 🛠️ Prerequisites & Setup

To deploy and host this workflow successfully, ensure you configure the following integrations:

1. **n8n Environment**: An operational instance of n8n (Cloud or self-hosted Docker).
2. **Google Cloud Console Credentials**:
   * Setup an OAuth2 credential with access scoped to `Google Drive API` and `Google Sheets API`.
3. **Google AI Studio API Key**:
   * Generate a valid API secret key from Google AI Studio to authenticate the **Google Gemini Node**.
4. **Target Spreadsheet**:
   * Create a Google Sheet containing headers matching your workflow data mapping (e.g., `Date`, `File Name`, `AI Summary`).

---

## 📥 How to Import to n8n

1. Copy the raw JSON code of your workflow from your source n8n setup.
2. Open your target n8n instance canvas.
3. Use the shortcut `Ctrl + V` (Windows) or `Cmd + V` (Mac) to paste the workflow directly onto the grid.
4. Open the **Google Drive**, **Google Sheets**, and **Gemini** nodes to link your active credentials.
5. Save and click **Execute Workflow**.
