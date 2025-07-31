Project Goals & Requirements Summary

1. Project Vision

The primary vision is to create an intelligent, self-hosted application that automates the organization and analysis of a personal collection of digital documents and images. The system will run as a containerized service on a Network-Attached Storage (NAS) device, providing a modern, web-based user interface accessible from any device on the local network. The core goal is to transform a static folder structure into a dynamic, searchable, and context-rich knowledge base.

2.  Core Functional Requirements

    Automated Document Ingestion: The system must automatically monitor designated "inbox" folders for new documents (initially PDF and DOCX files) and initiate a processing workflow without user intervention.

    Intelligent Content Analysis: For each document, the system will use a locally-run Large Language Model (Ollama) to:

        Generate a concise summary of the document's content.

        Extract a list of relevant keywords and descriptive tags.

        Create vector embeddings to represent the document's semantic meaning.

    Metadata & Context Storage: All generated context (summaries, tags, keywords, embeddings) and file metadata (filepath, creation date) must be stored in a structured database (ContextDB). This database will serve as the central index for all search and organizational activities, maintaining a clear link to the physical file on the NAS.

    Rule-Based Sorting: The application must be able to automatically move files from the inbox to a structured folder hierarchy based on file metadata, such as the document's creation date.

3.  User-Facing Features (Web GUI)

The application must provide a rich, interactive web interface with the following capabilities:

    File & Folder Navigation: Users must be able to browse their file system in a familiar, intuitive file-explorer-style view.

    Contextual Preview: When a user selects a file, the interface must instantly display all associated context from the database (summary, tags, keywords) in a dedicated preview pane.

    Advanced Search Capabilities: The interface must support multiple modes of searching:

        By Filename: Standard text search for file and folder names.

        By Keyword/Tag: Searching for all documents associated with specific keywords or tags.

        Vector (Semantic) Search: Searching for documents based on the conceptual meaning of a query, not just exact keywords.

    Dynamic Clustering: When a user clicks on a tag or keyword in the preview pane, the file browser should instantly filter its view to show all other documents that share that same tag, effectively displaying a "cluster" of related content.

4. Future Scope & Extensibility

The architecture must be designed to accommodate future enhancements, including:

    Image & Photo Management: Extending the analysis engine to process images, using multimodal models to generate descriptions and tags.

    Face Recognition: Integrating specialized libraries to identify and cluster photos based on the faces they contain, storing this information in a dedicated "FacesDB."
