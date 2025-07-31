High-Level Technical Design

This document outlines the technical architecture for the Ollama-Powered Document Analysis and Sorting Application. The system is designed as a modern, containerized web application, separating the backend processing from the frontend user interface to ensure a responsive and scalable experience.

1. Core Architectural Components

The application consists of three primary, decoupled components running within a single Docker container environment:

    The Backend Engine: A persistent Python script that acts as the core data processor.

        Responsibilities:

            File System Watcher: Continuously monitors designated input directories for new or modified files using a library like watchdog.

            Processing Pipeline: For each new file, it orchestrates the analysis workflow: text/data extraction, metadata reading, and dispatching to the Ollama service.

            Database Population: Responsible for writing all extracted text, generated metadata (summaries, tags), and vector embeddings into the ContextDB.

        Technology: Standard Python, watchdog library.

    The Web Server & API: A high-performance web server that acts as the bridge between the user's browser and the application's data.

        Responsibilities:

            Serve the static files (HTML, CSS, JS) for the frontend web application.

            Provide a well-defined RESTful API for the frontend to query the ContextDB.

            Offer WebSocket endpoints to push real-time updates to the frontend (e.g., notifying the UI when a new file has been successfully analyzed).

            Handle user requests for search, clustering, and context viewing by querying the database.

        Technology: FastAPI (Python web framework), Uvicorn (ASGI server).

    The Frontend Application: A modern, browser-based Single-Page Application (SPA) that provides the complete user interface.

        Responsibilities:

            Render the file browser, search bars, and context preview panes.

            Manage all user interactions (clicks, searches).

            Communicate with the backend via HTTP requests to the API and listen for real-time updates via WebSockets.

            Dynamically update the UI based on the data received from the backend without requiring page reloads.

        Technology: Standard HTML, CSS, and JavaScript. No complex frontend framework (like React or Vue) is necessary for the initial version to maintain simplicity.

2.  Data Management

    ContextDB: A SQLite database file stored within the container's persistent volume.

        Purpose: Acts as the "single source of truth" for all file metadata and generated context. It is optimized for fast lookups, which is critical for the UI's responsiveness.

        Schema: Will include tables for documents, tags, keywords, and potentially faces in the future, with clear relationships defined between them.

    File System: The application will interact directly with the host's file system, which will be mounted as a volume into the Docker container. This allows the application to read and move files on the NAS.

3.  Containerization & Deployment

    Docker: The entire application (Backend Engine, Web Server, Python environment) will be packaged into a single Docker image.

    Docker Compose: A docker-compose.yml file will be used to define the service, manage environment variables (like folder paths), and mount the necessary host volumes for documents and the database. This ensures easy, one-command deployment on the NAS.
