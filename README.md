# System Architecture Overview
This project is a microservices-based video-to-audio conversion platform. Users can upload videos, which are then converted to MP3 audio files and made available for download. Notifications are sent upon completion.

![pic](ProjectArchitecture.png)

🧩 Components Breakdown
🔐 /login Service
Handles user authentication.

Issues a JWT token upon successful login.

Interacts with PostgreSQL to store and retrieve user data.

📤 /upload Service
Accepts video files from users.

Stores video binary data temporarily.

Pushes video processing metadata into the RabbitMQ Video queue.

📨 Message Queues – RabbitMQ
Video Queue: Receives messages for incoming video uploads.

MP3 Queue: Sends video jobs to the Converter service for processing.

🎛️ Converter Service
Listens to the MP3 queue.

Converts uploaded videos into MP3 format.

Stores the MP3 binary data in MongoDB.

📥 /download Service
Allows users to download the converted MP3 files.

Retrieves data directly from MongoDB.

🔔 Notification Service
Sends push notifications when the audio file is ready.

Optionally sends email alerts to the user.

🗃️ Data Storage

Data Type	Service	Storage
User Data	Login	PostgreSQL
Video Binary	Upload Service	Temp Storage
MP3 Binary	Converter	MongoDB
🔄 Workflow Summary
User logs in → Receives JWT.

Uploads video via /upload → Stored + queued for processing.

RabbitMQ sends video data to Converter.

Converter processes and stores the MP3 file in MongoDB.

Notification service informs the user.

User downloads the MP3 via /download.

  

