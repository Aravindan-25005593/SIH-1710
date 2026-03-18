# Smart India Hackathon Workshop
# Date:18.3.2026
## Register Number:212224243001
## Name:ARAVINDAN SD
## Problem Title
SIH 1710: Enhancing Navigation for Railway Station Facilities and Locations
## Problem Description
Background: Railway stations are complex environments with numerous facilities and locations such as ticket counters, platforms, restrooms, food courts, and waiting areas. Passengers often face difficulties in navigating these spaces, especially in large or unfamiliar stations. Efficient and user-friendly navigation systems are crucial for improving passenger experience, reducing congestion, and ensuring timely travel connections. Description: The problem involves developing a comprehensive navigation solution for railway stations that assists passengers in locating various facilities and destinations within the station premises. This includes creating detailed maps, providing real-time directions, and integrating features such as accessibility options for individuals with disabilities. The solution should be intuitive, easy to use, and accessible via multiple platforms, including mobile devices and digital kiosks. Key challenges include updating navigation information in real-time, ensuring accuracy, and accommodating the diverse needs of all passengers. Expected Solution: The expected solution is a multi-platform navigation system that provides detailed, real-time directions to all facilities and locations within a railway station. This system should include: A mobile application with 3D interactive maps and step-by-step navigation. Digital kiosks located throughout the station with touch-screen interfaces. Voice-guided navigation for visually impaired passengers. Regular updates to reflect changes in station layout and facility locations. Integration with existing railway apps and services for seamless user experience. The solution should enhance the overall passenger experience by reducing confusion, saving time, and improving accessibility within the station.

## Problem Creater's Organization
Ministry of Railway

## Idea
The "RailPath Vision" project proposes an Infrastructure-Light, Intelligence-Heavy approach. Most indoor navigation fails because GPS signals are blocked by concrete and steel. Our solution uses Hybrid Localization—fusing Bluetooth Low Energy (BLE) beacons, Wi-Fi RTT (Round Trip Time), and the smartphone's internal sensors (accelerometer/gyroscope) to create a "Blue Dot" experience accurate to within 1 meter.

The system doesn't just show a map; it understands the context of the passenger. If a train is departing in 5 minutes, the UI shifts to "Express Mode," highlighting the fastest route to the specific coach position.

## Proposed Solution / Architecture Diagram
The architecture is divided into four distinct layers to ensure scalability across thousands of Indian railway stations:

1. The Data & Mapping Layer (The Foundation)
Digital Twin Creation: We use IMDF (Indoor Mapping Data Format) to create a multi-level digital twin of the station. This separates the ground floor (General Booking) from the Concourse and the Platforms.

Spatial Database: Utilizing PostgreSQL with PostGIS, we store every facility (ATVMs, Water Vending Machines, Escalators) as geographic coordinates.

2. The Positioning Layer (The "GPS" Replacement)
Sensor Fusion: We implement an Extended Kalman Filter (EKF) that combines:

BLE Trilateration: Fixed beacons provide absolute position.

Pedestrian Dead Reckoning (PDR): Uses the phone's step-counter to move the dot between beacon pings.

Visual Markers: QR codes at entry points to "re-calibrate" the user's starting position.

3. The Middleware/API Layer
Real-time Synchronization: This layer connects to the COA (Control Office Application) and ICMS (Integrated Coaching Management System) APIs. If a platform change occurs, the middleware triggers a rerouting event to all users heading to that train.

4. The Interface Layer (User Touchpoints)
Mobile App: A React Native/Flutter app featuring AR (Augmented Reality) overlays. Users point their camera at the floor, and 3D arrows appear, guiding them to their coach.

Smart Kiosks: High-performance web apps on touchscreens that allow "Send to Phone" functionality via QR code for a seamless transition from kiosk to mobile.

## Use Cases
Case A: The "Divyangjan" (Accessibility) Route
A passenger with physical disabilities enters the station. Upon selecting "Accessible Mode," the algorithm ignores all staircases and identifies a path consisting strictly of ramps and functional lifts. If a specific lift is reported as "Under Maintenance" in the system, the app dynamically reroutes them to the next nearest accessible point.

Case B: Integrated Coach Guidance
Indian trains are long (24+ coaches). Finding "S-7" or "B-1" is a struggle. Our solution integrates the Coach Position System. The app guides the user not just to the platform, but to the exact yellow stripe on the platform where their specific coach will stop.

Case C: Emergency Evacuation & SOS
In case of a fire or security alert, the system switches to "Emergency Mode." It pushes a high-priority notification to all active devices, displaying the nearest emergency exit. For the Railway Protection Force (RPF), it provides a "Heat Map" of passenger density to manage crowd control effectively.

## Technology Stack
The Mobile & Frontend Ecosystem
At the user-facing level, the system utilizes Flutter or React Native for the mobile application to ensure a single codebase serves both Android and iOS users while maintaining high performance for UI rendering. For the Digital Kiosks, a React.js or Next.js web application is deployed, optimized for large touch-screen interfaces and integrated with Electron to run as a standalone desktop-like application on the kiosk hardware. The mapping visuals are powered by Mapbox GL JS or Google Maps Platform (Indoor), which allows for the layering of custom 3D station floor plans over standard geographical data. To provide an immersive experience, Unity 3D combined with ARCore (Android) and ARKit (iOS) is used to implement Augmented Reality (AR) navigation, placing virtual directional arrows directly onto the user's camera feed.

The Positioning & IoT Layer
Since GPS is unreliable indoors, the core positioning logic relies on a Hybrid Localization Engine. This involves BLE 5.0 (Bluetooth Low Energy) beacons from manufacturers like Estimote or Minew, which broadcast signals for trilateration. The software side uses Sensor Fusion Algorithms, specifically Extended Kalman Filters (EKF), to combine BLE data with Pedestrian Dead Reckoning (PDR) from the smartphone’s built-in accelerometer, gyroscope, and magnetometer. For accessibility, the stack includes Google Text-to-Speech (TTS) and Web Speech API to provide real-time voice guidance for visually impaired passengers, ensuring the "Divyangjan" features are responsive and natural-sounding.

Backend Infrastructure & Data Management
The "brain" of the system is built using Node.js or Go (Golang) due to their ability to handle thousands of concurrent API requests with low latency. The spatial data, such as platform coordinates and facility locations, is managed by a PostgreSQL database with the PostGIS extension, which is the industry standard for geographic information systems (GIS). Real-time updates, such as platform changes or emergency alerts, are pushed to devices using WebSockets (Socket.io) or Firebase Cloud Messaging (FCM). To integrate with existing railway systems, the backend communicates with CRIS (Centre for Railway Information Systems) APIs via secure RESTful endpoints or gRPC for high-speed data exchange.

Cloud, DevOps, and Analytics
The entire solution is hosted on AWS or Azure, utilizing Lambda for serverless processing of location pings and S3 for storing high-resolution 3D assets and station maps. For offline-first capabilities, Couchbase or SQLite is used on the mobile device to cache station maps, ensuring navigation continues even if the passenger loses cellular signal deep within the station. Finally, a Data Analytics Pipeline using ELK Stack (Elasticsearch, Logstash, Kibana) is implemented to provide the Ministry with "heat maps" of passenger flow, helping station managers identify bottlenecks and improve crowd management during peak hours

## Dependencies
Hardware Installation Permission: Success depends on the Ministry of Railways allowing the mounting of small, non-intrusive BLE beacons on station pillars.

API Integration: Deep integration with the CRIS (Centre for Railway Information Systems) database is required to pull real-time platform updates and coach compositions.

Connectivity (Offline-First): Many platforms have poor data reception. The app must have an "Offline Map" feature where the station layout and routing logic are downloaded the moment the user enters the station Wi-Fi zone.

Maintenance Protocol: A system is needed to alert station staff if a beacon's battery is low or if a kiosk becomes unresponsive, ensuring 99.9% uptime.
