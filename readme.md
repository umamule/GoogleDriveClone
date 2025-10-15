# 🚀 Google Drive Clone (Full-Stack File Storage System)
A secure, cloud-based file storage application built to replicate core Google Drive functionality, enabling users to manage, upload, and download files and folders.

## ✨ Features
User Authentication: Secure user registration and login using JWTs and bcrypt.js.

File CRUD: Upload, delete, and download files with size restrictions (max 10MB).

Folder Management: Create and organize files within dedicated folders.

Cloud Storage: Integration with Cloudinary for scalable and efficient file hosting and delivery.

Forced Downloads: Backend logic to enforce file downloads (even for PDFs) using Cloudinary's fl_attachment flag.

Responsive UI: Clean, modern interface built with React and custom SCSS mixins for mobile support.

State Management: Global state management handled via React Context API (implied) and component-level state.

## 🛠️ Tech Stack
This project is built using a modern full-stack architecture:

Frontend
Technology	Description

React	Core library for building the user interface.

Vite	Fast build tool and development server.

Axios	HTTP client for API communication.

React Router	Handling client-side routing and URL parameters (folderId).

Sass/SCSS	Styling with custom mixins (@mixin mobile) for responsiveness.

React Toastify	Notifications for user feedback (uploads, deletions, downloads).

Backend

Technology	Description

Node.js	Runtime environment.

Express.js	Web application framework for building REST APIs.

MongoDB	NoSQL database for storing user, folder, and file metadata.

Mongoose	MongoDB object data modeling (ODM) for Node.js.

JWT & bcrypt	Secure authentication and password hashing.

External Services

Cloudinary: Primary file storage and CDN delivery.

## ⚙️ Setup and Installation


Prerequisites

Node.js (v18+)

MongoDB Instance (Local or Atlas)

Cloudinary Account (API Key, Secret, and Cloud Name)

Steps
Clone the Repository:

Bash

git clone <repository-url>

cd google-drive-clone

Backend Setup:

Bash

cd backend
npm install
Create a .env file in the /backend directory and add your environment variables:

PORT = 5000

MONGO = mongodb+srv://root:root@cluster0.mvhavn8.mongodb.net/

KEY = drivesecurekey


Frontend Setup:


cd frontend

npm install

npm run dev

The application will typically be accessible at http://localhost:5173.

## 💡 Key Code Highlights
Forced File Download Logic
The backend dynamically modifies the Cloudinary URL to ensure documents like PDFs are downloaded instead of being viewed inline:

JavaScript

// /backend/controller/file.js
// ...
let fileUrl = fileData.file;

const uploadPath = '/upload/';

if (fileUrl.includes(uploadPath)) {

    // Inserts 'fl_attachment/' into the Cloudinary URL path
    const index = fileUrl.indexOf(uploadPath) + uploadPath.length;
    fileUrl = fileUrl.slice(0, index) + 'fl_attachment/' + fileUrl.slice(index);
}

return res.redirect(fileUrl); // Forces browser to download

Frontend Download Trigger

The React component initiates the download by opening the backend route, which handles the secure redirect:

JavaScript

// /frontend/src/components/File.jsx
const handleDownload = (fileId, fileName) => {
    const downloadUrl = `${url}/files/downloadFile/${fileId}`;
    window.open(downloadUrl, '_blank');
    // Toast notification for user feedback...
};
