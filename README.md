## Here is what you will learn from this repo :)

- [WebRTC](#webrtc)
- [Peer-to-Peer (P2P) Networking](#peer-to-peer-p2p-networking)
- [Signalling and STUN Servers](#signalling-and-stun-servers)
- [TURN Server](#turn-server)
- [ICE Candidates](#ice-candidates)
- [Session Description Protocol](#session-description-protocol)
- [RTCPeerConnection](#rtcp-eerconnection)
- [SFU (Selective Forwarding Unit) Architecture in WebRTC](#sfu-selective-forwarding-unit-architecture-in-webrtc)
- [Why SFU](#why-sfu)
- [Other Architectures](#other-architectures)
- [Simulcast](#simulcast)

# WebRTC

WebRTC (Web Real-Time Communication) is a technology that enables **direct, real-time communication** (like audio, video, and data sharing) between web browsers or applications without needing servers for the communication itself.

# Peer-to-Peer (P2P) Networking

**Peer-to-peer (P2P)** is a way of connecting devices where they communicate directly with each other, without needing a central server in between. In a P2P setup, every device (or "peer") can act both as a client and a server, sharing resources like files, data, or even processing power.

### Example in real life:

Think of a group project where everyone shares their notes directly with one another, instead of uploading them to a shared folder first.

### Why it's useful:

- **Efficiency**: No bottleneck at a central server.
- **Scalability**: Adding more peers doesn’t slow things down as much.
- **Decentralization**: No single point of failure, so the system is harder to break.

P2P is commonly used in applications like file sharing (e.g., torrents), cryptocurrency (e.g., Bitcoin), and communication tools (like WebRTC for video calls). It’s all about devices working together directly, making the process faster and more efficient!

> You do need a central server for signalling and sometimes for sending media as well (turn).
> 

# Signalling and STUN Servers

### **Signaling Server**

A **signaling server** is like a matchmaker in WebRTC. It helps two devices (peers) find and connect to each other by exchanging the necessary connection setup information, such as:

- **SDP (Session Description Protocol)**: Describes the media formats and connection details.
- **ICE candidates**: Possible network routes the peers can use to connect.

The signaling server is only used during the connection setup (not for the actual media or data transfer). Once the connection is established, the peers communicate directly, and the signaling server is no longer needed.

**Example**: It’s like a middleman helping two people exchange phone numbers so they can call each other directly.

---

### **STUN Server**

A **STUN (Session Traversal Utilities for NAT)** server helps peers discover their **public IP address** and **port** when they’re behind NAT (Network Address Translation). This is important for enabling direct peer-to-peer communication, especially when one or both devices are on private networks.

- Without STUN, peers wouldn’t know how to reach each other’s public network location.
- It’s lightweight and only used to resolve network information, not for relaying data.

**Example**: Imagine you’re inside a building (private network), and you ask the receptionist (STUN server) for your building’s public address so someone outside can find you.

Check [https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/](https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/)

![https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fac6c5d61-a704-4ff5-89e5-5d8d887082e1%2FScreenshot_2024-05-04_at_4.57.40_PM.png?table=block&id=d8b50e61-842c-41e2-857c-ef2753af011f&cache=v2](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fac6c5d61-a704-4ff5-89e5-5d8d887082e1%2FScreenshot_2024-05-04_at_4.57.40_PM.png?table=block&id=d8b50e61-842c-41e2-857c-ef2753af011f&cache=v2)

![https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F3dc43212-493b-44bc-8fb4-7f6d55b16413%2FScreenshot_2024-05-04_at_5.07.21_PM.png?table=block&id=9294085b-6462-43d7-bde4-24deed58f9b8&cache=v2](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F3dc43212-493b-44bc-8fb4-7f6d55b16413%2FScreenshot_2024-05-04_at_5.07.21_PM.png?table=block&id=9294085b-6462-43d7-bde4-24deed58f9b8&cache=v2)

# **Turn server**

A lot of times, your network doesn’t allow media to come in from `browser 2` . This depends on how restrictive your network is

Since the `ice candidate` is discovered by the `stun server`, your network `might` block incoming data from `browser 2` and only allow it from the `stun server`

![https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F13065925-b2f1-44cf-88d9-714431244257%2FScreenshot_2024-05-04_at_5.13.50_PM.png?table=block&id=2ff90658-4bea-440e-bd07-c6fe791dbb52&cache=v2](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F13065925-b2f1-44cf-88d9-714431244257%2FScreenshot_2024-05-04_at_5.13.50_PM.png?table=block&id=2ff90658-4bea-440e-bd07-c6fe791dbb52&cache=v2)

### **Offer**

The process of the first browser (the one initiating connection) sending their `ice candidates` to the other side.

### **Answer**

The other side returning their `ice candidates` is called the answer.

# Session Desciption protocol

SDP (Session Description Protocol) is like a **"handshake document"** in WebRTC. It’s a text-based format that describes the **connection setup details** for audio, video, and other data streams between two peers.

### What does SDP do?

It answers questions like:

1. **What media** is being shared? (e.g., audio, video, or both)
2. **How will it be shared?** (e.g., codecs and formats)
3. **Where should it be sent?** (e.g., IP addresses and ports)

### Example:

When two peers want to set up a WebRTC call:

1. Peer A sends its SDP offer:
    - "Hi, I want to send video using codec H.264 and audio using codec Opus to this address and port."
2. Peer B responds with its SDP answer:
    - "Got it! I'll send video using H.264 and audio using Opus to your address and port."

### Key Information in an SDP:

- **Media types**: Audio, video, or data.
- **Codecs**: Format of audio/video (e.g., Opus, H.264).
- **Network info**: IP address, ports, and transport protocol (e.g., UDP or TCP).
- **Bandwidth limits**.

In short, **SDP is the "negotiation script"** that helps two peers agree on how to communicate in a WebRTC session!

# RTCPeerConnection

---

An **RTC Peer Connection** is the heart of WebRTC. It’s what makes it possible for two devices (or peers) to connect directly and share things like audio, video, or even data. Think of it as the tool that handles everything behind the scenes to set up and maintain the connection.

### What does it actually do?

1. **Sets up the connection**:
It helps your device and the other peer figure out how to communicate, like agreeing on the connection details (using signaling).
2. **Handles audio, video, or data**:
Once connected, it takes care of sending and receiving media streams (e.g., your voice and video during a call) or even files or messages.
3. **Adjusts to network conditions**:
If your internet connection fluctuates, the RTC Peer Connection works to keep the communication smooth by tweaking things like video quality or audio bitrate.

### How it works:

- You start by creating a connection object (`RTCPeerConnection`).
- The two peers exchange necessary information (like their connection addresses and supported media formats) through signaling.
- Once everything is set, the peer connection kicks in, and audio/video/data starts flowing directly between the devices.

    

### Example in action:

Imagine you’re making a video call with your friend. The peer connection is what’s enabling the call—it’s figuring out the best way to send your video and voice to your friend’s device and receive theirs in return. It also keeps everything in sync, even if the network conditions change.

---

# SFU (Selective Forwarding Unit) Architecture in WebRTC

Mental image: 

P2P is good while chatting or having conference call among 3 or 4 people. If there are total of 5 people, you would be sending data to 4 and the receiving data from 4 people. This would be the same for all people in the conference call. Suppose the number of people was 100. Sending and receiving data from such a large number of machines require much larger bandwidth support(which your machine do not have). What would be a better way? SFU!

The **SFU (Selective Forwarding Unit)** architecture is a scalable approach for managing WebRTC connections, especially for group audio/video calls or real-time conferencing. Unlike a simple peer-to-peer model, an SFU acts as a smart server that forwards media streams between participants.

---

### How SFU Works:

1. **Peer Sends Media to SFU**:
    
    Each participant (peer) sends their audio/video streams to the SFU server instead of sending them directly to every other participant.
    
2. **SFU Distributes Streams**:
    
    The SFU selectively forwards media streams to other participants based on factors like:
    
    - **Bandwidth**: Optimizes streams for each participant's network conditions.
    - **Layout Requirements**: For example, if a user is only viewing a specific speaker, the SFU might forward just that stream.
    - **Subscriptions**: Allows participants to subscribe only to the streams they want to receive.
3. **Peers Receive Streams**:
    
    Each participant receives the streams they need from the SFU, rather than getting all streams directly from every other peer.
    

---

### Advantages of SFU:

1. **Scalability**:
    - In a peer-to-peer mesh network, each participant sends their media stream to every other participant, which doesn’t scale well as the number of participants grows.
    - SFUs solve this by reducing the load on individual peers and centralizing stream distribution.
2. **Efficiency**:
    - Peers only upload one copy of their media stream to the SFU, regardless of the number of participants.
    - The SFU handles stream distribution, optimizing bandwidth usage for all peers.
3. **Flexibility**:
    - The SFU can perform advanced tasks like simulcasting (sending multiple versions of a stream at different qualities), prioritizing active speakers, and handling recording or streaming to external services.

---

### Disadvantages of SFU:

1. **Server Dependency**:
    - Unlike pure peer-to-peer connections, SFU introduces a server that could become a single point of failure if not properly managed.
2. **Additional Latency**:
    - Forwarding streams through an SFU adds a small delay compared to direct peer-to-peer connections.
3. **Infrastructure Costs**:
    - Running an SFU involves hosting and managing server resources, which can be expensive as the number of participants increases.

---

### SFU vs. Other Architectures:

| **Feature** | **SFU** | **MCU (Multipoint Control Unit)** | **P2P Mesh** |
| --- | --- | --- | --- |
| **Scalability** | High | Medium | Poor |
| **Bandwidth Usage** | Low for peers, moderate for the server | High for the server | High for peers |
| **Processing Load** | Light (selective forwarding only) | Heavy (mixing all streams) | Heavy for peers |
| **Latency** | Low to moderate | High (due to media mixing) | Low |
- **P2P Mesh**: Every peer connects directly to every other peer (works best for small groups).
- **MCU (Multipoint Control Unit)**: The server mixes all streams into one unified stream, which simplifies processing for peers but increases server load and latency.
- **SFU**: Balances efficiency and scalability, making it ideal for medium to large group calls.

---

### Use Cases for SFU:

- **Video Conferencing**: Apps like Zoom and Google Meet use SFU for efficient group calls.
- **Live Streaming**: Forwarding high-quality streams to multiple viewers in real time.
- **Gaming & Collaboration Tools**: For low-latency interaction between multiple users.

---

### Example in Action:

Imagine a 5-person video call using SFU:

1. Each participant sends **one video stream** to the SFU.
2. The SFU selectively forwards the streams:
    - **Active speaker's video** goes to all participants.
    - Lower-quality streams go to users with limited bandwidth.
    - If a participant mutes video, their stream isn’t forwarded to others.

This design keeps bandwidth low while maintaining high quality and a smooth experience for everyone!

# Why SFU

Mental image: 

P2P is good while chatting or having conference call among 3 or 4 people. If there are total of 5 people, you would be sending data to 4 and the receiving data from 4 people. This would be the same for all people in the conference call. Suppose the number of people was 100. Sending and receiving data from such a large number of machines require much larger bandwidth support(which your machine do not have). What would be a better way? SFU!

The need for **SFU (Selective Forwarding Unit)** arose because other architectures like **peer-to-peer mesh** and **MCU (Multipoint Control Unit)** had significant limitations when scaling up for group communication. Here’s a breakdown of why SFU became essential:

---

### 1. **Peer-to-Peer Mesh: Bandwidth and Scaling Issues**

In a peer-to-peer mesh network:

- **How it works**: Every participant connects directly to every other participant, sending their media stream to each peer individually.
- **Problem**: Bandwidth usage grows exponentially as the number of participants increases.
    - For example, in a 5-person call, each participant sends **4 separate streams** and receives **4 streams**—a total of 8 streams per person.
    - This becomes unsustainable for larger groups due to bandwidth limitations, especially for participants on slower networks.
- **CPU Load**: Each peer processes multiple incoming and outgoing streams, overwhelming devices in larger calls.

**Need for SFU**: SFU centralizes the routing of streams, so participants only upload one stream, solving the bandwidth and CPU issues.

---

### 2. **MCU (Multipoint Control Unit): Server Overload**

In an MCU architecture:

- **How it works**: The MCU server collects all incoming streams, mixes them into a single unified stream, and sends it back to all participants.
- **Problem**:
    - **High Server Load**: The MCU does heavy processing to mix streams, increasing infrastructure costs significantly.
    - **Latency**: Mixing streams takes time, leading to higher delays in real-time communication.
    - **Bandwidth**: The server bears the entire bandwidth load of both receiving and sending streams, which doesn’t scale well with large groups.

**Need for SFU**: SFU avoids media mixing, forwarding streams as-is with minimal processing, making it far more efficient.

# Other Architectures

While **SFU**, **P2P Mesh**, and **MCU** are the most commonly used architectures for WebRTC-based communication, there are emerging architectures or hybrid models that offer **enhanced scalability, performance, and flexibility** depending on specific use cases. Here are a few other architectures or improvements that could potentially offer better alternatives in certain scenarios:

---

### 1. **Hybrid SFU-MCU**

- **Overview**: This architecture blends **SFU** and **MCU** elements to offer more flexibility in handling different use cases within the same system. For example, an SFU can be used for most peer-to-peer stream forwarding, while an MCU can be employed for specific tasks like mixing streams for a lower bandwidth participant or when combined media output is needed.
- **When It's Better**:
    - When you need the best of both worlds: scalability of SFU with the mixing capabilities of MCU.
    - For applications where specific participants need combined media (e.g., speakers or hosts) while others can benefit from selective forwarding.
- **Example**: Some **telemedicine platforms** or **large-scale events** may combine these two approaches to meet different user needs.

---

### 2. **Cloud-Based Microservices Architecture**

- **Overview**: Instead of relying on a single SFU or MCU, this model uses **microservices** deployed in the cloud to manage individual media streams and routing. It divides the workload across multiple specialized services that are responsible for different tasks such as **streaming, transcoding**, **bandwidth optimization**, and **recording**.
- **When It's Better**:
    - When you need **extreme scalability** for applications involving large amounts of traffic or data, such as **live streaming platforms** with millions of concurrent users.
    - Useful in **cloud-native environments** where you want to scale different components independently.
- **Example**: Platforms like **Twitch**, **YouTube Live**, or **Facebook Live** that need to manage millions of viewers and ensure low latency with minimal server resources.

---

### 3. **Mesh + Distributed Network (Decentralized)**

- **Overview**: This model pushes the peer-to-peer concept further by leveraging **distributed networks** and **blockchain** technologies. In a truly decentralized WebRTC network, the peers themselves could manage media forwarding and routing (with some assistance from lightweight nodes or relays), potentially eliminating the need for centralized servers like SFUs or MCUs.
- **When It's Better**:
    - When you require **complete decentralization**, reducing the need for centralized servers and offering **peer-to-peer control**.
    - Ideal for applications like **Web3 video conferencing** or **peer-to-peer file sharing** where decentralization and security are key concerns.
- **Example**: Emerging **decentralized social platforms** or **Web3-based communication tools**.

---

### 4. **SVC (Scalable Video Coding) with SFU**

- **Overview**: While not an architecture in itself, **SVC (Scalable Video Coding)** can be integrated with an SFU to enhance video quality by allowing a single stream to be transmitted at multiple resolutions or quality levels. The SFU can then forward different quality versions to different participants based on their network conditions, optimizing bandwidth usage and video quality.
- **When It's Better**:
    - When you want **adaptive video streams** without the complexity of multiple resolutions for each user.
    - Especially useful in scenarios with varying network conditions or different types of devices.
- **Example**: Video conferencing applications that want to provide **adaptive bitrate streaming** without overwhelming the server.

---

### 5. **WebRTC Mesh + TURN Servers for NAT Traversal**

- **Overview**: Though **TURN servers** are commonly used to assist with NAT traversal (allowing peers behind firewalls to connect), some advanced systems integrate **TURN with mesh architectures** for specific tasks like **video relay** in congested networks. In this model, TURN helps route media traffic only when necessary (e.g., if direct P2P isn’t feasible), enhancing connectivity without entirely moving away from the P2P model.
- **When It's Better**:
    - When your application has strict requirements for **peer-to-peer connectivity** and you want to minimize reliance on centralized servers.
    - Useful in **low-latency** applications where you want to preserve the performance of direct peer connections but still ensure reliable connectivity in restricted networks.
- **Example**: Real-time applications like **gaming** or **peer-to-peer file sharing**, where direct peer connections are ideal but sometimes TURN is needed for NAT traversal.

---

### 6. **Application Layer Optimization (End-to-End Encryption with SFU)**

- **Overview**: This architecture places a stronger focus on **end-to-end encryption** and security within WebRTC systems. It allows encryption at the application level, which is then combined with an SFU that securely forwards encrypted streams between peers without decrypting them. This adds a layer of privacy and security to the existing SFU model.
- **When It's Better**:
    - When your application requires **enhanced security** and you need to ensure **privacy and compliance** (e.g., **healthcare, finance**).
    - Ideal for sensitive real-time communications where **end-to-end encryption** is essential.
- **Example**: Secure **telemedicine platforms** or **financial video consultations**.

---

### Conclusion: Is There a "Better" Architecture?

Each WebRTC architecture has strengths and weaknesses, and the "best" solution depends on your specific use case:

- **SFU** remains the go-to solution for most real-time communication applications due to its **scalability**, **low latency**, and **bandwidth efficiency**.
- If your needs go beyond the capabilities of SFU, exploring **hybrid models** (SFU-MCU, SVC with SFU) or **cloud-based microservices** could offer better results in highly specialized or large-scale systems.
- For decentralization or **Web3-based applications**, exploring **peer-to-peer mesh with distributed networks** or **blockchain solutions** can provide cutting-edge, decentralized approaches to real-time communication.

Ultimately, the "better" architecture depends on factors like **scalability**, **latency requirements**, **security**, and **infrastructure costs**.

# Simulcast

Do you know? In a google meet, when you pin someone’s video ( the guy is zoomed in to the page), for a few milliseconds, his video quality would be 360p (bad quality) and then it suddenly becomes high quality. This is because of Simulcast.

Simulcast is the mechanism in which a machine is sending video or audio in multiple qualities so that the other machine at the receiving end can use the required quality for his needs(ex. 360p video quality when he is not pinned in google meet. and 720p when one actually gets pinned and cover the screen).

**Simulcast** is a technique used in real-time communication (like WebRTC) where a single video stream is encoded and sent at multiple different **resolutions** or **bitrates**. This allows the server or SFU (Selective Forwarding Unit) to send different versions of the stream to different participants based on their **network conditions** or device capabilities.

### Key Points:

- **Multiple Streams**: A participant sends a video stream in multiple quality levels (e.g., low, medium, high resolution).
- **Adaptive**: The SFU or server can choose which resolution to forward to each participant, optimizing **bandwidth usage**.
- **Improved Experience**: This ensures that users with lower bandwidth receive a lower-quality stream, while those with better connections can receive a higher-quality version, improving overall call quality for all participants.

### Example:

In a group video call, a participant might send a high-quality 1080p stream, but the SFU will forward a lower 360p version to users with slower internet speeds, maintaining smooth communication for everyone.
