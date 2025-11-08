# 🖥️ Remote Desktop System with MongoDB Integration

<div align="center">

![Java](https://img.shields.io/badge/Java-11+-007396?style=for-the-badge&logo=java&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Database-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-Build_Tool-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Swing](https://img.shields.io/badge/Java_Swing-GUI-007396?style=for-the-badge&logo=java&logoColor=white)

**A powerful Java-based remote desktop application with real-time screen sharing, remote control, and integrated chat**

[Features](#-features) • [Architecture](#-architecture) • [Installation](#-installation) • [Usage](#-usage)

</div>

---

## 🌟 Overview

This Remote Desktop System is a comprehensive Java application that enables users to remotely access and control computers over a network. Built with Java Swing for the GUI and MongoDB for data persistence, it provides a seamless remote desktop experience with additional features like real-time chat, file transfer, and session management.

---

## ✨ Features

### 🖥️ **Remote Desktop Control**
- **Real-Time Screen Sharing** - Live screen capture and streaming with JPEG compression
- **Remote Input Control** - Control mouse and keyboard remotely (requires control grant)
- **High-Performance Capture** - Optimized screen capture using Java Robot API
- **Adjustable Quality** - Configurable JPEG quality (default 0.7) and frame rate (15-120 FPS)
- **Auto FPS Adjustment** - Dynamically adjusts frame rate based on network performance
- **Multi-Monitor Support** - Captures all screens in multi-monitor setups
- **Control Management** - Server can grant/revoke control to specific clients

### 💬 **Integrated Communication**
- **Real-Time Chat** - Built-in chat system for client-server communication
- **Chat History** - Persistent message storage in MongoDB via ChatMessageDAO
- **Broadcast Messaging** - Messages sent to all connected clients
- **Server-Client Chat** - Bidirectional communication between host and viewers

### 📁 **File Transfer**
- **Bidirectional Transfer** - Send and receive files between client and server
- **Multiple File Types** - Support for all file formats
- **Automatic Directory Creation** - Creates download directories automatically
- **File Broadcasting** - Files sent to all connected clients
- **Server Downloads** - Files saved to `server_downloads/` directory

### 🗄️ **Database Integration**
- **MongoDB Backend** - NoSQL database for data persistence
- **Session Management** - Track and log all remote sessions
- **Activity Logging** - Comprehensive audit trail of all actions
- **User Management** - Store and manage user credentials
- **User Preferences** - Save and restore user settings

### 🔐 **Security Features**
- **Password Authentication** - Server password protection for connections
- **Username Verification** - Unique username requirement (no duplicates)
- **Session Tracking** - All sessions logged with start/end times in MongoDB
- **Activity Logging** - Comprehensive audit trail via ActivityLogDAO
- **User Management** - Password hashing with UserDAO
- **IP Address Logging** - Track client IP addresses for security auditing

**Note**: Encryption (SSL/TLS) is not currently implemented but recommended for production use.

### 🎨 **User Interface**
- **Dual-Mode Interface** - Separate client and server GUIs
- **Tabbed Navigation** - Easy switching between client and server modes
- **Responsive Design** - Adaptive UI for different screen sizes
- **System Look & Feel** - Native OS appearance
- **Status Indicators** - Real-time connection status

---

## 🏗️ Architecture

### **Client-Server Model**
```
┌─────────────┐         Network         ┌─────────────┐
│   Client    │ ◄─────────────────────► │   Server    │
│   (Viewer)  │    Socket Connection    │  (Host)     │
└─────────────┘                         └─────────────┘
      │                                        │
      │                                        │
      ▼                                        ▼
┌─────────────┐                         ┌─────────────┐
│  Input      │                         │  Screen     │
│  Handler    │                         │  Capturer   │
└─────────────┘                         └─────────────┘
      │                                        │
      └────────────────┬───────────────────────┘
                       ▼
                ┌─────────────┐
                │   MongoDB   │
                │  Database   │
                └─────────────┘
```

### **Component Structure**

#### **Client Components**
- `ClientGUI.java` - Main client interface
- `Client.java` - Client connection logic
- `ScreenViewer.java` - Display remote screen
- `InputHandler.java` - Handle user input
- `ChatPanel.java` - Client-side chat interface

#### **Server Components**
- `ServerGUI.java` - Main server interface
- `Server.java` - Server connection management
- `ClientHandler.java` - Handle individual client connections
- `ScreenCapturer.java` - Capture and stream screen
- `ServerChatPanel.java` - Server-side chat interface
- `ChatManager.java` - Manage chat sessions

#### **Common Components**
- `Constants.java` - Application-wide constants
- `Message.java` - Message data structure
- `FileTransfer.java` - File transfer utilities
- `MongoDBConnection.java` - Database connection manager

#### **Database Models**
- `User.java` - User entity
- `Session.java` - Session tracking
- `ChatMessage.java` - Chat message entity
- `ActivityLog.java` - Activity logging
- `UserPreferences.java` - User settings

#### **Database Access Objects (DAOs)**
- `UserDAO.java` - User data operations
- `SessionDAO.java` - Session management
- `ChatMessageDAO.java` - Chat persistence
- `ActivityLogDAO.java` - Activity logging
- `UserPreferencesDAO.java` - Preferences management

---

## 🛠️ Tech Stack

### **Core Technologies**
- **Java 11+** - Primary programming language
- **Java Swing** - GUI framework
- **Java AWT Robot** - Screen capture and input control
- **Java Sockets** - Network communication
- **Java I/O** - File operations and streaming

### **Database**
- **MongoDB 4.11+** - NoSQL database
- **MongoDB Java Driver** - Database connectivity
- **BSON** - Binary JSON for data storage

### **Build & Deployment**
- **Maven** - Dependency management and build automation
- **Launch4j** - Windows executable (.exe) generation
- **Maven Shade Plugin** - Create fat JAR with dependencies

---

## 📦 Installation

### Prerequisites
- **Java Development Kit (JDK) 11 or higher**
- **Maven 3.6+**
- **MongoDB 4.0+** (local or cloud instance)

### Setup Instructions

1. **Clone the Repository**
   ```bash
   git clone https://github.com/VibhorSurana03/Remote_Desktop_System.git
   cd Remote_Desktop_System
   ```

2. **Start MongoDB**
   
   Ensure MongoDB is running on your system:
   ```bash
   # On Windows (if installed as service)
   net start MongoDB
   
   # On macOS/Linux
   mongod
   ```
   
   **Optional**: Configure MongoDB Connection
   
   Edit `src/common/database/MongoDBConnection.java` if you need to change defaults:
   ```java
   private static final String CONNECTION_STRING = "mongodb://localhost:27017";
   private static final String DATABASE_NAME = "remoteDesktopDB";
   ```

3. **Build the Project**
   ```bash
   mvn clean package
   ```
   
   This will:
   - Compile all Java sources
   - Create a fat JAR with all dependencies in `target/` directory
   - Generate `RemoteDesktop.exe` (Windows only, requires Launch4j)
   
   **Note**: The application will continue to run even if MongoDB connection fails, but database features will be unavailable.

4. **Run the Application**
   
   **Option 1: Using JAR**
   ```bash
   java -jar target/remotedesktopsystem-1.0-SNAPSHOT.jar
   ```
   
   **Option 2: Using Executable (Windows)**
   ```bash
   RemoteDesktop.exe
   ```
   
   **Option 3: Using Maven**
   ```bash
   mvn exec:java -Dexec.mainClass="Main"
   ```

---

## 🎯 Usage

### Server Setup (Host Computer)

1. **Launch the Application**
   - Run the application using any of the methods above
   - Switch to the "Server" tab

2. **Start the Server**
   - Click "Start Server" button
   - The server will begin listening on the default port (default: 5000)
   - Note the server IP address displayed

3. **Wait for Connections**
   - The server is now ready to accept client connections
   - Monitor incoming connections in the server panel

### Client Setup (Remote Computer)

1. **Launch the Application**
   - Run the application
   - Switch to the "Client" tab

2. **Connect to Server**
   - Enter the server IP address
   - Enter the port number (default: 5000)
   - Click "Connect"

3. **Remote Control**
   - Once connected, you'll see the remote screen
   - Use your mouse and keyboard to control the remote computer
   - Use the chat panel to communicate
   - Transfer files using the file transfer feature

### Features Usage

#### **Screen Sharing**
- Automatically starts when client connects
- Adjust quality settings in preferences
- Toggle full-screen mode for better viewing

#### **Chat**
- Type messages in the chat input field
- Press Enter or click Send
- View chat history in the message panel

#### **File Transfer**
- Click "Send File" button
- Select file from file chooser
- Monitor transfer progress
- Received files are saved to default download location

---

## ⚙️ Configuration

### Network Settings
- **Default Port**: 5000 (configurable in `Constants.java`)
- **Buffer Size**: 8192 bytes
- **Timeout**: 30 seconds

### Screen Capture Settings
- **Default Resolution**: Full screen (all monitors combined)
- **Frame Rate**: 30 FPS default (adjustable 15-120 FPS range)
- **Auto FPS Adjustment**: Enabled by default (can be disabled)
- **Image Format**: JPEG
- **Compression Quality**: 0.7 default (adjustable via Constants.JPEG_QUALITY)

### Database Settings
- **Connection String**: `mongodb://localhost:27017` (configurable in `MongoDBConnection.java`)
- **Database Name**: `remoteDesktopDB`
- **Connection Pool**: Max 20, Min 5 connections
- **Collections**: users, sessions, chatMessages, activityLogs, userPreferences
- **Indexes**: Automatically created for optimal query performance

---

## 🔒 Security Considerations

- **Network Security**: Use VPN or secure network for remote connections
- **Authentication**: Implement strong password policies
- **Encryption**: Consider adding SSL/TLS for production use
- **Firewall**: Configure firewall rules to allow only trusted connections
- **Audit Logs**: Regularly review activity logs in MongoDB

---

## 🚀 Future Enhancements

- [ ] SSL/TLS encryption for secure communication
- [ ] File transfer progress indicators
- [ ] Typing indicators for chat
- [ ] Audio streaming
- [ ] Clipboard synchronization
- [ ] Session recording and playback
- [ ] Mobile client support
- [ ] Web-based interface
- [ ] Cloud deployment support
- [ ] Advanced authentication (2FA, OAuth)
- [ ] Bandwidth optimization and adaptive quality
- [ ] Screen region selection (partial screen capture)
- [ ] Multiple simultaneous client control

---

## 🐛 Troubleshooting

### Connection Issues
- Verify server is running and listening
- Check firewall settings
- Ensure correct IP address and port
- Verify network connectivity

### MongoDB Connection Errors
- Ensure MongoDB is running
- Check connection string
- Verify database permissions
- Check MongoDB logs

### Performance Issues
- Reduce screen capture quality
- Lower frame rate
- Check network bandwidth
- Close unnecessary applications

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

---

## 📝 License

This project is open source and available under the MIT License.

---

## 👨‍💻 Developer

**Vibhor Surana**

- GitHub: [@VibhorSurana03](https://github.com/VibhorSurana03)
- Email: vibhorsurana03@gmail.com

---

<div align="center">

**⭐ Star this repository if you find it useful!**

*Connecting computers, empowering remote work* 🌐💻

Made with ❤️ by Vibhor Surana

</div>
