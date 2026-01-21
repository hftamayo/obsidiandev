### 1. Project Structure (The Pipeline Layout)
We’ll stick to the Go standard, but we'll add a `generator/` layer specifically for the document engines.

```
reporting-service/
├── cmd/
│   └── server/             # Entry point
├── internal/
│   ├── contract/           # The "Reporting Contract"
│   │   └── models.go
│   ├── generator/          # Logic for different formats
│   │   ├── generator.go    # Interface (Export())
│   │   ├── pdf_gen.go      # Maroto or Gofpdf logic
│   │   ├── xlsx_gen.go     # Excelize logic
│   │   └── json_gen.go     # Standard encoding
│   ├── worker/             # Job queue management
│   │   └── pool.go         # Handles heavy lifting
│   └── websocket/          # Hub/Client (from our template)
├── pkg/
│   └── docutils/           # Styling, Fonts, Templates
└── go.mod
```


### 2. The Reporting Contract (`models.go`)
As you requested, we keep it simple but include a `Format` field so the service knows what the "printer" should output.

```textmate
type ReportRequest struct {
    Timestamp     time.Time   `json:"timestamp"`
    Project       string      `json:"project"`
    System        string      `json:"system"`
    Module        string      `json:"module"`
    RequestedUser string      `json:"requestedUser"`
    Format        string      `json:"format"` // "pdf", "xlsx", "json"
    Data          interface{} `json:"data"`   // The payload to be formatted
}
```

### 3. Architectural Highlights
*   **Worker Pool Pattern:** Generating PDFs and Excel files is CPU-intensive. Instead of doing it directly in the WebSocket loop, we’ll pass the request to a **Worker Pool**. This ensures the WebSocket stays responsive even if a 500-page report is being generated.
*   **Streaming Output:** For large reports, the service can stream the generated file back to the client or provide a temporary signed URL for download, preventing memory exhaustion.
*   **Template-Based Rendering:** We’ll use Go's `html/template` or a dedicated PDF DSL to ensure the "Project" and "Module" headers look professional and consistent across all reports.
*   **Asynchronous Job Tracking:** Since document generation takes time, the service will provide a `JobUUID` upon request. The client will receive real-time status updates via WebSocket (Pending -> Processing -> Completed), mirroring a real-world print queue.
*   **Ephemeral Storage Management:** Generated files will be treated as temporary assets. The service will implement an automated cleanup strategy to purge files after a successful download or a defined expiration period (e.g., 1 hour), ensuring the system remains lean.* 

### 4. Technology Stack (Go Ecosystem)
*   **Maroto (or Gofpdf):** For PDF generation. Maroto is excellent because it uses a "grid-based" layout similar to Bootstrap, making it very easy to code layouts.
*   **Excelize:** The gold standard in Go for reading and writing XLSX files. It’s fast and handles massive datasets efficiently.
*   **Gorilla WebSocket:** (Same as Logger) To receive the report request and send back status updates (e.g., "Processing...", "Done: [URL]").

### 5. Suggested Design Patterns
*   **Factory Pattern:** We’ll use a **Generator Factory**. Based on the `Format` field in the contract ("pdf", "xlsx"), the factory will return the correct implementation of the `Generator` interface.
*   **Strategy Pattern:** Similar to the Logger's storage, each output format is a strategy. Adding a new format (like CSV or HTML) in the future won't require changing the core WebSocket logic.

### 6. Operational Considerations (The "Printer" Logic)
*   **Statelessness:** Unlike the Logger which uses Redis for sessions, the Reporter can be largely stateless. It receives data, converts it, and hands it back.
*   **TDD:** We'll test the "Engines" separately. We can create unit tests that feed the `pdf_gen.go` a JSON payload and verify that a valid PDF byte-stream is produced.

