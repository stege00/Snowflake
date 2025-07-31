Supporting Technical Documentation

1. Preliminary Database Schema (ContextDB)

This is a potential schema for our SQLite database. It's designed to be simple but extensible.

documents table:

    id (INTEGER, PRIMARY KEY)

    filepath (TEXT, UNIQUE, NOT NULL) - The absolute path to the file on the NAS.

    filename (TEXT, NOT NULL)

    creation_date (DATETIME)

    last_modified (DATETIME)

    summary (TEXT)

    raw_text (TEXT) - Storing the extracted text can prevent re-processing.

    embedding (BLOB) - To store the vector embedding.

tags table:

    id (INTEGER, PRIMARY KEY)

    name (TEXT, UNIQUE, NOT NULL)

document_tags table (Junction Table):

    document_id (INTEGER, FOREIGN KEY to documents.id)

    tag_id (INTEGER, FOREIGN KEY to tags.id)

(A similar structure would be used for keywords.)

2. Preliminary API Endpoints

This outlines some of the core API endpoints the FastAPI server would provide.

    GET /api/files/

        Action: Get the list of all files and folders to display in the file browser.

        Response: A JSON structure representing the file tree.

    GET /api/context/{filepath}

        Action: Retrieve all context for a specific file.

        Response: JSON object containing summary, tags, keywords, etc.

    GET /api/search/

        Action: Perform a search.

        Query Parameters: q=<query>, type=<keyword|filename|semantic>

        Response: A list of file paths that match the search criteria.

    GET /api/cluster/{tag_name}

        Action: Get all files associated with a specific tag.

        Response: A list of file paths.

    WebSocket Endpoint WS /ws/updates

        Action: A persistent connection where the backend can push real-time status updates to the frontend (e.g., "File 'NewDoc.pdf' has been processed.").
