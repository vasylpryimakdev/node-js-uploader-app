# Node.js File Uploader App

A simple TCP-based file uploader application built with Node.js. This project demonstrates client-server communication for file transfers with progress tracking.

## Overview

This application consists of two main components:

- **Server**: Listens for incoming connections and receives files
- **Client**: Connects to the server and uploads files with progress indication

## Features

- 🚀 Fast TCP-based file transfer
- 📊 Real-time upload progress tracking
- 💾 Automatic file storage in dedicated directory
- 🔄 Backpressure handling for optimal performance
- 📝 Clean console output with progress updates

## Project Structure

```
uploader-app/
├── server.js          # TCP server that receives files
├── client.js          # TCP client that uploads files
├── storage/           # Directory where uploaded files are saved
├── testing.txt        # Sample file for testing uploads
└── README.md          # This file
```

## Installation

1. Clone or download this project

```bash
git clone https://github.com/vasylpryimakdev/node-js-uploader-app.git
```

2. Navigate to the project directory

```bash
cd node-js-uploader-app
```

3. No additional dependencies required - uses only Node.js built-in modules

## Usage

### Starting the Server

```bash
node server.js
```

The server will start listening on `::1:5050` and display:

```
Uploader server opened on { address: '::1', family: 'IPv6', port: 5050 }
```

### Uploading Files

```bash
node client.js <file-path>
```

Example:

```bash
node client.js testing.txt
```

The client will:

- Connect to the server
- Display upload progress (0% to 100%)
- Show success message when complete

### Testing with Sample File

```bash
# Start server in one terminal
node server.js

# Upload the test file in another terminal
node client.js testing.txt
```

## How It Works

### Server (`server.js`)

- Creates a TCP server on port 5050
- Handles multiple client connections
- Parses file headers to extract filenames
- Implements backpressure control with `socket.pause()`/`resume()`
- Saves files to the `storage/` directory
- Properly closes file handles when connections end

### Client (`client.js`)

- Connects to the server using TCP
- Sends file metadata in the format: `fileName: <filename>-------`
- Reads files in chunks and streams them to the server
- Displays real-time upload progress
- Handles backpressure with stream pausing/resuming
- Provides visual feedback with cursor control

## Technical Details

- **Protocol**: Custom TCP protocol with simple header format
- **Backpressure**: Implemented using Node.js stream `drain` events
- **Progress Tracking**: Calculates percentage based on bytes transferred
- **File Handling**: Uses `fs/promises` for modern async file operations
- **UI**: Console-based progress display with cursor manipulation

## Requirements

- Node.js (version with `fs/promises` support)
- No external dependencies

## Notes

- The server stores all uploaded files in the `storage/` directory
- The `storage/` directory is created automatically if it doesn't exist
- Multiple concurrent uploads are supported
- The application handles large files efficiently through streaming
- IPv6 localhost (`::1`) is used by default for the connection

## Example Output

### Server:

```
New connection!
Connection ended!
```

### Client:

```
Uploading... 25%
Uploading... 50%
Uploading... 75%
Uploading... 100%
The file was successfully uploaded!
```
