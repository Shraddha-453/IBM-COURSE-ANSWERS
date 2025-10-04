# 1. Traditional App: New Note (Post/Redirect/Get)
```mermaid
sequenceDiagram
    participant browser
    participant server

    Note left of browser: User fills the text field and clicks 'Save'
    browser->>server: POST [https://studies.cs.helsinki.fi/exampleapp/new_note](https://studies.cs.helsinki.fi/exampleapp/new_note)
    activate server
    Note right of server: Server processes new note data and saves it.
    server-->>browser: HTTP status code 302 Found (Redirect to /notes)
    deactivate server
    
    Note left of browser: Browser follows redirect, starts fetching the updated page
    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/notes](https://studies.cs.helsinki.fi/exampleapp/notes)
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/main.css](https://studies.cs.helsinki.fi/exampleapp/main.css)
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/main.js](https://studies.cs.helsinki.fi/exampleapp/main.js)
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the updated JSON

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/data.json](https://studies.cs.helsinki.fi/exampleapp/data.json)
    activate server
    server-->>browser: [{ "content": "NEW NOTE", "date": "...", }, ... ]
    deactivate server

    Note right of browser: The browser executes the callback function that renders all notes, including the new one

# 2.Single page app diagram
```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/spa](https://studies.cs.helsinki.fi/exampleapp/spa)
    activate server
    server-->>browser: HTML document
    deactivate server

    Note right of browser: Browser starts rendering HTML

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/main.css](https://studies.cs.helsinki.fi/exampleapp/main.css)
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/spa.js](https://studies.cs.helsinki.fi/exampleapp/spa.js)
    activate server
    server-->>browser: the JavaScript file (SPA bundle)
    deactivate server

    Note right of browser: The SPA JavaScript executes and fetches the notes JSON

    browser->>server: GET [https://studies.cs.helsinki.fi/exampleapp/data.json](https://studies.cs.helsinki.fi/exampleapp/data.json)
    activate server
    server-->>browser: [{ "content": "SPA is fun", "date": "2024-1-1" }, ... ]
    deactivate server

    Note right of browser: The SPA dynamically renders the notes on the page

# 3. New note in Single page app diagram
    ```mermaid
    sequenceDiagram
    participant browser
    participant server

    Note left of browser: User fills the text field and clicks 'Save'
    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    activate server
    Note right of server: Server processes new note data and saves it.
    server-->>browser: HTTP status code 201 Created
    deactivate server

    Note right of browser: The JavaScript executes, fetching the updated notes JSON

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "New SPA Note", "date": "...", }, ... ]
    deactivate server

    Note right of browser: The SPA dynamically renders the new note without reloading the page
 
