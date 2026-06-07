# World Machine Model: Telegram Desktop


## 1 World

### 1.1 Users and Devices

#### 1.1.1 Human Actors

A User is a person who communicates with other persons or automated services through the Telegram network. Users send text messages, share media files, initiate voice and video calls, and participate in group conversations of any size. Users may be individuals communicating privately, members of large public communities, or operators of automated bot accounts. Each User holds a registered account tied to a verified phone number, and may optionally protect that account with a two-factor cloud password.

A single installation of the client may be configured for more than one account. The User operating the client switches between accounts without restarting the application. Each account maintains its own conversation history, settings, and authentication state independently of all other accounts on the same installation.

#### 1.1.2 Devices and Operating Systems

Telegram Desktop runs on personal computers. The supported operating systems are Windows, macOS, and Linux distributions that meet the minimum dependency requirements. On each operating system the application appears as a native desktop process, presenting a graphical window built from the host platform toolkit.

Each device provides the following capabilities to the application: a display with a windowing environment, a file system for local storage, audio capture and playback hardware, a camera or screen capture facility for video calls, a native notification subsystem, a system tray or status-bar area, and the ability to register global keyboard shortcuts. The availability of specific hardware such as a camera or microphone varies by device, and the application operates in a reduced-capability mode when hardware is absent.

The application enforces a single-instance constraint per data directory. If a second launch is attempted while one instance is already running, the second launch signals the first instance and exits.

#### 1.1.3 User Roles and Permissions

Within the Telegram network, Users hold different roles depending on the conversation type. In a private conversation, both participants are peers with equal message rights. In a basic group, any member may post messages, and designated administrators may change group settings. In a channel or supergroup, a channel owner grants posting and administration rights explicitly. These roles affect which operations the client is permitted to request from the server and which operations the server will accept.


### 1.2 Network Conditions

#### 1.2.1 Connectivity Varieties

Devices connect to the Telegram network over a variety of link types, including Wi-Fi, wired Ethernet, and cellular data networks ranging from 2G to 5G. Throughput, latency, and packet-loss characteristics differ substantially across these link types. A device may transition between link types during a session, causing the active connection to drop and requiring reconnection.

Users on metered connections may impose data usage limits. The client respects auto-download policies that restrict which media types are fetched automatically depending on the current connection type.

#### 1.2.2 Network Filtering and Proxies

In certain jurisdictions or network environments, direct connections to Telegram server endpoints may be blocked or throttled. Users may configure proxy servers to route traffic. The client supports SOCKS5 and HTTP proxy configurations. When direct connection fails, the client attempts alternate transport modes, including HTTP-based transport and TLS-wrapped connections that blend with generic HTTPS traffic.

The client can also resolve data-centre addresses through a DNS-based fallback mechanism when primary endpoint resolution fails. CDN edge addresses are provided by the server and do not require separate proxy configuration.

#### 1.2.3 Real-Time Media Connectivity

Voice and video calls require a real-time media channel separate from the main messaging channel. This media channel is established using STUN and TURN server infrastructure operated by Telegram. When peer-to-peer connectivity is possible, media flows directly between devices. When direct connectivity fails, media is relayed through Telegram relay servers. Call signaling messages travel through the standard server-assisted path before the media channel is open.

The latency and reliability requirements for real-time media are more stringent than those for message delivery. Packet loss and jitter directly affect call audio and video quality, and the call subsystem adapts its encoding and transmission strategy in response to changing link conditions.


### 1.3 External Services

#### 1.3.1 Telegram Server Infrastructure

Telegram Server is a distributed set of data centres, each identified by a numeric identifier. Data centres are grouped into production and test environments. The client connects to the data centre appropriate for its account's home region and may connect to additional data centres for upload, download, configuration, and other request types.

Each data centre exposes network endpoints over which the MTProto binary protocol is spoken. The server authenticates clients, stores message history and media metadata, routes messages between users, and pushes update notifications to connected clients.

The server may direct the client to transfer large media files through CDN Edges rather than through the primary server path. In this case the server provides encryption keys and locations for the CDN content, and the client fetches media parts directly from CDN infrastructure.

#### 1.3.2 CDN Edges

CDN Edges are content-delivery servers operated by third-party providers under Telegram direction. They serve encrypted media parts for download. The client receives the location and decryption parameters for CDN-served files from the Telegram Server. The client verifies the integrity of CDN-delivered content before storing it locally.

#### 1.3.3 Third-Party Services

Payment processors are external web services integrated into the Telegram payment flow. When a User interacts with a payment-capable message, the client opens a web view pointing to the payment processor's interface. The client treats these web views as partially trusted environments and prompts the User before granting access to account or payment data.

Web apps and games are also delivered as web views. These are third-party or Telegram-hosted HTML applications opened inside the client. The client enforces boundaries between the web view context and the main application and presents confirmation prompts before sensitive interactions.


### 1.4 Content and Media

#### 1.4.1 Text and Structured Messages

Users send plain text messages and messages with inline formatting. Inline formatting includes bold, italic, underline, strikethrough, monospace, links, and custom emoji. Messages may contain polls, where recipients vote on predefined options. Messages may carry geographic locations. Messages may carry contact cards.

Messages are associated with a conversation, a sender, a timestamp, and a server-assigned identifier. Messages can be edited after sending, and both the original and edited forms are visible in the conversation history. Messages can be deleted, optionally for all conversation participants.

#### 1.4.2 Media Types

Users send and receive the following media types: photos with size variants and thumbnails, videos with inline preview thumbnails, audio tracks, voice messages recorded as Opus-encoded audio, video messages recorded as short looping round clips, documents of arbitrary format and size, stickers in static and animated forms, and custom emoji used inline in message text.

Each media item may be large. Photos are provided in multiple resolution variants. Videos and large files may exceed hundreds of megabytes. The client may download media lazily and stores downloaded content in a local cache to avoid repeated downloads.

#### 1.4.3 Stickers and Animated Content

Sticker sets are collections of image or animation files associated with a user-visible pack name. Users install sticker sets and access recently used stickers from a dedicated panel. The client maintains registry of installed, recent, favourite, featured, and archived sets and synchronises this registry with the server.

#### 1.4.4 Message Interactions

Users react to messages with emoji reactions. Users forward messages from one conversation to another. Users reply to specific earlier messages within a conversation, creating a visual thread. Users pin messages within a conversation for persistent visibility at the top of the view. Users schedule messages for future delivery, and the server delivers them at the scheduled time.


### 1.5 Regulatory and Privacy Context

#### 1.5.1 Encryption Expectations

Users expect that ordinary cloud messages are stored encrypted on Telegram servers and accessible to the account on any authenticated device. Users expect that secret chat messages are encrypted device-to-device and are not accessible to Telegram or to any other device, including other devices of the same account.

Voice and video calls are always end-to-end encrypted. The encryption key for a call is derived through a Diffie-Hellman exchange and is verified by both participants through an out-of-band fingerprint displayed as an emoji sequence. Users who compare and confirm matching emoji fingerprints know that no call interception has occurred.

#### 1.5.2 Data Deletion and Self-Destruction

Users have the right to delete messages they have sent, and may choose to delete those messages for all participants in a conversation. Users may delete their entire account, which removes their data from Telegram servers.

Secret chat messages carry an optional self-destruction timer. After the recipient reads a message subject to a self-destruction timer, the message is deleted from both devices after the specified interval. The client enforces this timer locally and sends a corresponding deletion signal to the remote device.

#### 1.5.3 Privacy Controls

Users control their visibility to others through privacy settings. These settings govern who can see the User's last-seen status, profile photo, phone number, and online status. Users can restrict who may add them to groups, who may call them, and who may view their profile. These privacy settings are stored on the server and synchronised to the client.

#### 1.5.4 Local Data Protection

All data stored locally by the application is subject to protection by an optional passcode. When a passcode is set, the local encryption key is derived from that passcode. Without the correct passcode, locally stored authentication data, message cache, media cache, and settings are inaccessible. The application presents a lock screen when the passcode is active and the User is inactive.


## 2 Requirements

### 2.1 Account and Identity

#### 2.1.1 Phone Verification

The system shall require a User to provide a phone number to begin account registration or sign-in. The system shall send a verification code to the provided phone number. The system shall verify the code entered by the User before proceeding.

#### 2.1.2 Account Creation

The system shall allow a new User to provide a first name and optional last name during initial registration after phone verification succeeds. The system shall create a new Telegram account for the User upon successful sign-up.

#### 2.1.3 Cloud Password

The system shall support a two-factor cloud password on accounts that have enabled it. The system shall prompt the User to enter the cloud password during sign-in when the account has two-factor authentication active. The system shall not complete sign-in until the correct cloud password is provided or the password recovery flow is completed.

#### 2.1.4 Alternative Authentication

The system shall support sign-in by QR code displayed on screen, which another already-authenticated Telegram device scans to authorise the new session. The system shall support sign-in by passkey where the platform passkey subsystem is available.

#### 2.1.5 Session Management

The system shall allow the User to view all active authorised sessions for the account. The system shall allow the User to revoke any individual session. The system shall terminate the local session when the User chooses to log out.

#### 2.1.6 Multiple Accounts

The system shall allow more than one account to be added to a single installation. The system shall allow the User to switch the active account. The system shall maintain separate histories, settings, and authentication state for each account.


### 2.2 Messaging

#### 2.2.1 Message Delivery

The system shall deliver messages sent by the User to the Telegram Server and confirm local delivery state. The system shall display an unambiguous delivery indicator on each outgoing message, distinguishing pending, sent, delivered, and read states.

#### 2.2.2 Message Ordering

The system shall display messages in the order assigned by the server. The system shall not reorder messages after they are confirmed by the server.

#### 2.2.3 Read Receipts

The system shall mark incoming messages as read when the User views them in a conversation. The system shall transmit a read mark to the server so that the sender can observe delivery of the read state.

#### 2.2.4 Message Editing

The system shall allow the User to edit a previously sent message within the time window permitted by the server. The system shall display an edit indicator on messages that have been modified after sending.

#### 2.2.5 Message Deletion

The system shall allow the User to delete a sent message from the local view. The system shall allow the User to delete a sent message for all conversation participants when the User has the right to do so. The system shall remove deleted messages from local history display immediately after server confirmation.

#### 2.2.6 Forwarding and Replies

The system shall allow the User to forward one or more messages to another conversation. The system shall allow the User to reply to a specific message, and the reply shall reference the original message visually.

#### 2.2.7 Typing Indicators

The system shall transmit a typing action to the server while the User is composing a message, at intervals that avoid excessive network traffic. The system shall display a typing indicator in the conversation when the server reports that a remote participant is composing.

#### 2.2.8 Pinned Messages

The system shall display the currently pinned message in a conversation when one exists. The system shall allow authorised Users to pin and unpin messages.

#### 2.2.9 Scheduled Messages

The system shall allow the User to schedule a message for delivery at a future time. The system shall display the scheduled message locally as pending until the server confirms delivery at the scheduled time.

#### 2.2.10 Drafts

The system shall preserve an unsent draft for each conversation across application restarts. The system shall synchronise drafts to the server so that the draft is available on other devices of the same account.

#### 2.2.11 Reactions

The system shall allow the User to add or remove an emoji reaction on any message where the server permits reactions. The system shall display the current set of reactions and their counts on each message that carries them, updating the display immediately when the server confirms a reaction change.


### 2.3 Media Transfer

#### 2.3.1 File Upload

The system shall allow the User to select local files and send them as attachments in a message. The system shall upload the selected file to the Telegram Server before or concurrent with sending the message. The system shall report upload progress to the User.

#### 2.3.2 Resumable Upload

The system shall resume an interrupted upload from the last confirmed part rather than restarting from the beginning when network connectivity is restored.

#### 2.3.3 File Download

The system shall download media attachments and display them to the User. The system shall report download progress. The system shall allow the User to save a downloaded file to a chosen location on the local file system.

#### 2.3.4 Resumable Download

The system shall resume an interrupted download from the last successfully received chunk rather than restarting from the beginning when network connectivity is restored.

#### 2.3.5 Auto-Download Policy

The system shall apply per-account auto-download policies that specify which media types are downloaded automatically on which connection types. The system shall respect the User's configured limits and not download media that exceeds those limits without explicit User action.

#### 2.3.6 Streaming Playback

The system shall begin playback of audio and video media before the entire file has been downloaded, provided sufficient buffered data is available. The system shall continue downloading the remaining file in the background during playback.

#### 2.3.7 CDN Download

The system shall transparently download media from CDN Edges when the server directs it to do so, verifying content integrity before caching.


### 2.4 Voice and Video Communication

#### 2.4.1 Call Initiation

The system shall allow the User to initiate a voice or video call to another User. The system shall signal the call request through the Telegram Server to the remote party.

#### 2.4.2 Call Reception

The system shall alert the User to an incoming call. The system shall allow the User to accept or decline an incoming call.

#### 2.4.3 Call Encryption

The system shall encrypt the call media stream end-to-end using a key derived through a Diffie-Hellman exchange. The system shall display the Key Fingerprint as an emoji sequence to both participants so they can verify the key out-of-band.

#### 2.4.4 Group Calls

The system shall allow multiple participants to join a shared group call. The system shall allow the User to manage microphone mute state, video, and screen-sharing within a group call.

#### 2.4.5 Call Termination

The system shall allow the User to end a call at any time. The system shall transmit a discard reason to the server upon call termination. The system shall optionally prompt the User to rate a completed call.

#### 2.4.6 Call Quality Adaptation

The system shall adapt audio and video encoding parameters in response to measured network conditions during a call to maintain the best achievable quality.


### 2.5 Notifications

#### 2.5.1 Incoming Message Notifications

The system shall deliver a notification to the User when a new message arrives in a conversation that is not muted, including when the application is not in the foreground.

#### 2.5.2 Per-Peer Mute

The system shall allow the User to mute notifications for an individual conversation for a specified duration or indefinitely. The system shall suppress notifications for muted conversations.

#### 2.5.3 Global Mute

The system shall allow the User to mute all notifications globally. The system shall respect the global mute state and suppress all notifications while it is active.

#### 2.5.4 Quiet Hours

The system shall allow the User to configure a quiet-hours window during which notifications are suppressed. The system shall suppress notifications that would otherwise be delivered within the quiet-hours window.

#### 2.5.5 Notification Content

The system shall format the notification text to include the sender name and a message preview unless the User's privacy settings restrict preview display. The system shall suppress the message preview in notifications for secret chats.

#### 2.5.6 Platform Notification Integration

The system shall deliver notifications through the operating system's native notification subsystem where one is available. The system shall fall back to an in-application notification presentation when the native subsystem is unavailable or disabled.


### 2.6 Privacy and Security

#### 2.6.1 Secret Chats

The system shall support secret chats that are encrypted device-to-device with keys that are not held by Telegram servers. The system shall not store secret chat messages in the cloud. The system shall store secret chat history only on the devices that are party to the secret chat.

#### 2.6.2 Message Self-Destruction

The system shall enforce self-destruction timers on secret chat messages that carry them. The system shall delete a self-destructing message from local storage after the configured interval has elapsed following the recipient's first view. The system shall signal the deletion to the remote party.

#### 2.6.3 Privacy Settings

The system shall synchronise the User's privacy settings with the server. The system shall apply those settings to control the visibility of last-seen status, profile photo, and phone number to other users.

#### 2.6.4 Local Passcode

The system shall support an optional local passcode that locks the application on the local device. The system shall display a lock screen requiring the correct passcode before granting access to messages and account data. The system shall automatically lock the application after a configurable idle period.

#### 2.6.5 Trusted-Peer Prompts

The system shall prompt the User for confirmation before opening a payment interface, launching a game, or displaying a web app from an external source. The system shall record the User's trust decision and not repeat the prompt for trusted peers.


### 2.7 Offline and Synchronisation

#### 2.7.1 Outgoing Message Queue

The system shall queue outgoing messages locally when network connectivity is unavailable. The system shall transmit queued messages in order when connectivity is restored, without User intervention.

#### 2.7.2 Reconnection

The system shall automatically attempt to reconnect to the Telegram Server after a connection is lost, using an incremental back-off schedule. The system shall resume the update stream from the last known state after reconnection succeeds.

#### 2.7.3 Draft Synchronisation

The system shall synchronise message drafts to the server so that a draft composed on one device is available on other devices of the same account.

#### 2.7.4 History Synchronisation

The system shall fetch missing history updates using the difference mechanism when updates are detected to be out of sequence or absent after reconnection. The system shall apply fetched updates in the correct order before presenting them to the User.

#### 2.7.5 Unread Count Synchronisation

The system shall synchronise unread message counts and read marks with the server so that clearing a conversation on one device is reflected on other devices.


### 2.8 Platform Integration

#### 2.8.1 System Tray

The system shall display a tray icon in the operating system's tray or status area while the application is running. The system shall update the tray icon to reflect the presence of unread messages.

#### 2.8.2 Unread Badge

The system shall display an unread message count badge on the application icon in the operating system's task bar or dock. The system shall aggregate unread counts across all configured accounts.

#### 2.8.3 Global Shortcuts

The system shall register configurable global keyboard shortcuts for operations such as toggling the main window and activating push-to-talk in a call. The system shall respond to these shortcuts even when the application window is not focused.

#### 2.8.4 File Associations and URL Schemes

The system shall register as a handler for the Telegram URL scheme and for file types associated with the Telegram protocol. The system shall handle incoming scheme URLs by opening the appropriate conversation, user profile, or deep-link target.

#### 2.8.5 Single Instance

The system shall ensure that only one instance runs per data directory at any time. The system shall pass arguments from a second launch attempt to the running instance and bring that instance to the foreground.


## 3 Specifications

### 3.1 Session and Account Management

#### 3.1.1 Domain Container and Account Lifecycle

The Domain Container is the root owner of all configured accounts. On application startup, the Application Core creates the Domain Container and instructs it to load the account index from Domain Storage. For each account recorded in the index, the Domain Container creates an Account object and invokes its loading routine.

The Account loads its authentication data from Account Storage using the Local Key. If authentication data is valid and complete, the Account activates itself by creating a Session. If no valid authentication data is found, the Account presents the Intro Flow for the User to authenticate. Only one Account per window is active at a time. The Domain Container tracks which Account is active and coordinates switching when the User selects a different account from the account switcher.

#### 3.1.2 Authentication Sequence

The Intro Flow presents a sequence of Intro Steps to the User. The first step collects a phone number. The corresponding Intro Step sends a request through a Request Sender to the Protocol Engine, which routes it to the Telegram Server. On success, the Intro Flow advances to the code-entry step. The code-entry Intro Step sends a sign-in request with the received code. If the server reports that a cloud password is required, the Intro Flow advances to the cloud-password step. The cloud-password Intro Step uses the API Facade to retrieve the password configuration and then sends the derived password check request. On successful authentication, the Intro Flow signals the Account to create a Session.

The Protocol Engine performs a Diffie-Hellman key exchange via the Key Creator before the first authenticated request can be sent to a Data Centre. The Key Creator generates a new Authorization Key and registers it with the Data Centre entry in the Data Centre Registry. The Protocol Engine then binds the authorization using the Key Binder before any session-level messages are sent.

#### 3.1.3 Session Initialisation

When an Account creates a Session, the Session instantiates the Data Model, the API Facade, the Update Dispatcher, and Session Settings. The Session Settings load per-account preferences from Account Storage. The Data Model initialises empty and is populated progressively as the API Facade requests dialogs, contacts, and sticker sets from the server. The Update Dispatcher begins listening for updates delivered by the Protocol Engine.

#### 3.1.4 Account Switching and Shutdown

When the User switches to a different account, the Domain Container deactivates the currently active Account, which destroys its Session and releases in-memory data, and then activates the target Account. Account Storage for the deactivated Account is flushed before deactivation completes. On application shutdown, the Quit Coordinator waits for any in-progress uploads or downloads to reach a safe checkpoint before permitting the process to exit.

#### 3.1.5 Unread Badge Aggregation

The Domain Container subscribes to unread count change events from each Account. When any Account's unread count changes, the Domain Container recomputes the aggregate and instructs Platform Integration to update the application badge and tray icon.


### 3.2 MTProto Protocol Engine

#### 3.2.1 Protocol Engine Structure

The Protocol Engine is the single facade over all network communication with Telegram servers. It is owned by the Account and created during Account initialisation. The Protocol Engine owns the Data Centre Registry, which records endpoints, Authorization Keys, and CDN public keys for each Data Centre. The Protocol Engine creates and manages one Protocol Session per Shifted Data Centre Identifier in active use.

#### 3.2.2 Data Centre Selection

When a request must be sent to a specific Data Centre, the Protocol Engine looks up or creates the appropriate Protocol Session for the corresponding Shifted Data Centre Identifier. If no Authorization Key exists for the target Data Centre, the Protocol Engine directs the Key Creator to perform a Diffie-Hellman exchange with that Data Centre before any payload is sent. The RSA Public Key bundled with the application is used to encrypt the initial key-exchange payload.

#### 3.2.3 Authorization Key Creation

The Key Creator generates a Diffie-Hellman nonce, sends an initial handshake request to the server, receives the server's Diffie-Hellman parameters, computes the shared secret, derives the Authorization Key, and stores it in the Data Centre entry. If the Account uses temporary Authorization Keys, the Key Binder binds the temporary key to the persistent key before session traffic begins. The resulting Authorization Key encrypts all subsequent messages to that Data Centre.

#### 3.2.4 Protocol Session and Session Worker

Each Protocol Session owns a Session Worker that runs on a background thread. The Session Worker holds the active Transport Connection, sends queued requests, receives server responses, and performs AES-IGE encryption and decryption of message payloads. The Session Worker maintains message sequence numbers, acknowledgements, and server salts for its Protocol Session.

When a transport failure occurs, the Session Worker closes the current Transport Connection and creates a new one. The Protocol Engine selects the transport variant based on the configured connection mode and the current network reachability result. The available variants are raw TCP, HTTP-based transport, and TLS-wrapped connection. Connection through a proxy wraps whichever transport variant is selected.

#### 3.2.5 Request Dispatch and Response Handling

The API Facade sends requests by constructing typed request objects and passing them to the Protocol Engine. The Protocol Engine enqueues the request for the appropriate Protocol Session. The Session Worker serialises, encrypts, and transmits the request. On receiving a response, the Session Worker decrypts, parses, and delivers a Protocol Response or Protocol Error to the registered callback. The API Facade applies successful responses to the Data Model.

#### 3.2.6 Configuration and Endpoint Refresh

The Config Loader periodically fetches updated server configuration from a dedicated endpoint and from a special configuration server. Updated configuration may change Data Centre endpoints, add new CDN keys, or modify client behaviour flags. The Domain Resolver provides DNS-based address resolution as a fallback when primary endpoint connection fails.


### 3.3 Update Dispatcher

#### 3.3.1 Update Delivery Path

The Telegram Server pushes update messages to the client through the Protocol Engine. The Protocol Session for the primary Data Centre receives these updates and delivers them to the Update Dispatcher. The Update Dispatcher is owned by the Session and processes updates on the main thread.

#### 3.3.2 Sequence Tracking

The Update Dispatcher maintains a Sequence Tracker for the global update stream and one Sequence Tracker per Channel Entity that participates in channel-level update sequencing. Each Sequence Tracker holds the last known point-in-time counter, sequence counter, and query counter for its scope. When an incoming update carries a sequence number that follows contiguously from the last applied number, the Sequence Tracker approves immediate application. When a gap is detected, the Sequence Tracker suppresses application of the out-of-sequence update and triggers a Difference Request.

#### 3.3.3 Difference Requests

A Difference Request fetches the range of updates between the last known state and the current server state from the Telegram Server. The Update Dispatcher serialises pending updates received after a gap is detected and applies all fetched and pending updates in the correct order after the difference response is received. If the difference response itself indicates further gaps, the Update Dispatcher issues additional Difference Requests until the state is consistent.

#### 3.3.4 Update Application to Data Model

Once an update passes sequence validation, the Update Dispatcher routes it to the Data Model for application. Each update type modifies the appropriate Data Model entity: new message updates create or update Message Items in the relevant History, peer updates modify Peer entities, read-mark updates change History unread state, and so on. After applying each update, the Data Model emits change events through the Change Notifier.


### 3.4 Data Model and Cache

#### 3.4.1 Data Model Ownership

The Data Model is owned by the Session and is the single authoritative in-memory cache for all entities in one account. It holds all Peers, Histories, Message Items, Media Entities, and related objects. No other subsystem caches these entities independently. UI components and the API Facade query the Data Model and hold references to its objects.

#### 3.4.2 Peer Management

The Data Model owns all Peer objects, which are specialised as User Entity, Chat Entity, and Channel Entity. Peers are uniquely identified by a server-assigned numeric identifier. The Data Model creates a Peer entry on first encounter and updates it when the server sends updated peer data. Peers are not deleted from the Data Model during a session unless the User explicitly leaves or removes a conversation.

Each Channel Entity owns its own Sequence Tracker, which the Update Dispatcher uses for channel-level update sequencing independently of the global sequence.

#### 3.4.3 History and Message Management

Each Peer has a corresponding History owned by the Histories Registry. The History holds an ordered collection of Message Items indexed by server-assigned message identifier. The Histories Registry coordinates message sending requests, read-mark transmission, and dialog entry operations across all Histories.

When the User sends a message, the History first creates a Message Item with a local identifier and pending status. Upon server confirmation, the Message Item is updated with the server-assigned identifier and confirmed status. This two-step process ensures the message appears in the UI immediately, before server confirmation.

#### 3.4.4 Media Entity Management

The Data Model caches Photo Entities and Document Entities by their server-assigned identifier. When the same photo or document is referenced by more than one message, all references point to the same Media Entity object. A Media View held by a consumer controls the lifetime of the loaded bytes for that Media Entity in the local cache, preventing eviction while the consumer holds the view.

#### 3.4.5 Reactive Change Notifications

The Change Notifier is the event publication hub for the Data Model. Subsystems that display or process Data Model entities subscribe to relevant change event types through the Change Notifier. When the Data Model applies an update, it calls the Change Notifier, which dispatches typed events to all registered subscribers. The Dialogs List, the conversation view, and the Notification Center are primary subscribers.

#### 3.4.6 Stickers and Emoji Registry

The Stickers Registry is the Data Model component that tracks all sticker and custom emoji sets. It holds separate collections for installed sets, recently used stickers, favourite stickers, featured sets, and archived sets. The API Facade synchronises the Stickers Registry with the server when the session is established and when the User modifies sticker set preferences.

#### 3.4.7 Group Call Registry

The Group Call Registry is the Data Model component that holds active Group Call objects keyed by call identifier. When a User joins a group call, the Call Engine looks up the Group Call in the registry or creates it. Other subsystems access the Group Call through the registry rather than holding direct references.

#### 3.4.8 Reactions Registry

The Reactions Registry is the Data Model component that owns the catalogue of available Reaction types for the account. Each Reaction describes one emoji that the server permits users to place on messages, together with its display properties. The Data Model loads the Reactions Registry from the server at session start and refreshes it when the server signals a change to the permitted set.

Each Message Item may carry a collection of Reactions representing the current reaction state of that message as last reported by the server. Each entry in this collection identifies the Reaction type, the total count of users who placed that reaction, and whether the local User is among them. When the server delivers a reaction update for a message, the Update Dispatcher applies the new reaction state to the corresponding Message Item in the Data Model. The Data Model then notifies subscribed components by emitting a message change event through the Change Notifier. The Session Controller, which owns the active conversation view, receives this event and redraws the reaction strip on the affected message. The API Facade sends a reaction add or remove request to the server when the User interacts with the reaction strip, and the Data Model updates the Message Item optimistically before server confirmation arrives.


### 3.5 File Download and Streaming

#### 3.5.1 Download Scheduler

The Download Scheduler is the central coordinator for all file download activity in a Session. It maintains a priority queue of Download Tasks, one per active file location. The Download Scheduler allocates a Shifted Data Centre Identifier per Data Centre for download traffic, keeping download requests on separate Protocol Sessions from interactive requests. The scheduler limits the number of concurrent part requests per data centre and adjusts the limit based on measured throughput.

#### 3.5.2 Download Task and File Loader

Each Download Task corresponds to one file and is driven by a File Loader. The File Loader checks the Local Cache Database before issuing any network requests. If the requested file is fully present in the cache, the File Loader delivers the cached bytes directly. Otherwise, the File Loader requests the missing parts from the network.

An MTProto File Loader requests parts through the Protocol Engine. If the server redirects the file to a CDN Edge, the MTProto File Loader switches to CDN-based fetching, verifying hash integrity of each received part before writing it to the Local Cache Database. If a file reference expires during download, the MTProto File Loader refreshes the reference through the API Facade and retries.

A Web File Loader downloads a file over plain HTTP using the platform HTTP client, writing the result to the Local Cache Database on completion.

#### 3.5.3 Local Cache Database

The Local Cache Database is an encrypted key-value store on disk. It is partitioned into a small-file store for thumbnails and inline images and a big-file store for large documents and media. Both partitions are encrypted with the Local Key. Downloaded parts are written to the big-file store incrementally. Completed small media items are written to the small-file store. On cache lookup, the File Loader provides the media key and receives the stored bytes if present.

#### 3.5.4 Streaming Reader and Loader

The Streaming Reader serves byte ranges of a media file on demand. It holds a map of already-cached slices obtained from the Local Cache Database and issues on-demand part requests through a Streaming Loader for any uncached range. The Streaming Loader uses the same Protocol Engine path as the MTProto File Loader. The Streaming Reader does not require the full file to be present before serving initial byte ranges.

#### 3.5.5 Streaming Player and Streaming Instance

The Streaming Player receives byte ranges from a Streaming Reader, decodes audio and video frames using the media decoder, and drives synchronized audio and video output. A Streaming Instance is the client handle created for one consumer of a Streaming Player. The consumer receives decoded frame events and playback state transitions through the Streaming Instance. Multiple Streaming Instances of the same Player are not permitted.

The Streamed File Downloader is a variant that drives a Streaming Reader to download and cache a file to disk without playback, used to save a media file to the User's chosen file system location.

#### 3.5.6 Audio Player

The Audio Player is a shared subsystem that manages audio tracks and short sound effects across the application. It receives decoded audio from the Streaming Player or from preloaded buffers and sends it to the platform audio output. The Audio Player manages a playback queue and supports pause, seek, and volume control.

#### 3.5.7 Download History Manager

The Download History Manager records all completed and in-progress downloads in a persistent store. It exposes this list in the application downloads panel. On application restart, the Download History Manager restores in-progress download state so that the Download Scheduler can resume interrupted downloads.


### 3.6 File Upload Pipeline

#### 3.6.1 File Selection and Preparation

When the User selects one or more files to send as an attachment, the API Facade creates a File Preparation Task for each file and submits it to the Task Queue. The Task Queue runs File Preparation Tasks on a background worker thread, away from the main thread. Each File Preparation Task reads the file from disk, generates thumbnail images, computes media dimensions and duration where applicable, determines the target media type, and produces an upload-ready descriptor. For images and videos, the File Preparation Task may optionally compress or re-encode the content according to the User's quality settings.

#### 3.6.2 Upload Execution

On completion of a File Preparation Task, the Upload Manager receives the upload-ready descriptor. The Upload Manager splits the file content into fixed-size parts. It selects an upload Shifted Data Centre Identifier and issues parallel part requests through the Protocol Engine to the assigned Data Centre, up to the allowed concurrency limit. Each successfully received part acknowledgement is recorded by the Upload Manager. Progress is reported to the UI through the Change Notifier.

If a part request fails, the Upload Manager retries that part. If the upload session expires, the Upload Manager restarts the upload from the beginning. If connectivity is lost, the Upload Manager suspends upload activity and resumes when connectivity is restored, continuing from the last confirmed part.

#### 3.6.3 Message Dispatch After Upload

The Upload Manager signals completion to the API Facade after all parts of a file have been confirmed by the server. The API Facade then constructs the message send request, embedding the server-assigned media reference returned by the upload confirmation. The message send request is sent through the Protocol Engine. The History creates a pending Message Item before the request is sent, and the Message Item is confirmed when the server acknowledges the message.


### 3.7 Voice and Video Call Engine

#### 3.7.1 Call Engine Ownership

The Call Engine is owned by the Application Core and thus is not bound to any single Session or Account. It creates and destroys One-to-One Call and Group Call objects, routes incoming call updates to the correct call object, and enforces the constraint that only one call may be active at a time across all accounts.

#### 3.7.2 One-to-One Call Signaling

When the User initiates a voice or video call, the Call Engine creates a One-to-One Call object and instructs the API Facade to send a call-request message to the server. The server relays the request to the remote peer and returns the initial call parameters. The One-to-One Call performs a Diffie-Hellman exchange to derive the encryption key, using parameters provided by the server. The server mediates the exchange by relaying the Diffie-Hellman payloads between the two clients through the Signaling Relay mechanism.

When the remote party accepts the call, the One-to-One Call transitions to the key-confirmation state. Both clients independently compute the Key Fingerprint from the derived key and display it as an emoji sequence. If both parties confirm matching emoji, the key is verified. The One-to-One Call then creates the Media Controller and enters the active state.

#### 3.7.3 Media Controller and Call Libraries

The Media Controller is the abstract interface for the real-time media subsystem. The One-to-One Call creates either a WebRTC Controller or a TgVoip Controller depending on the negotiated call parameters. The WebRTC Controller integrates with the WebRTC-based call library, which handles ICE candidate gathering, STUN and TURN interaction, DTLS handshake, and SRTP-based media encryption above the Diffie-Hellman-derived call key. The TgVoip Controller integrates with the legacy voice-over-IP library.

The Media Controller receives audio from the platform capture subsystem and delivers decoded audio to the platform playback subsystem. If video is enabled, the Media Controller additionally handles video capture from camera or screen and video rendering. The Call Delegate provides the Media Controller with access to the configured video capture source and the call sound effects playback facility.

#### 3.7.4 Group Call Operation

A Group Call is managed by the Group Call Registry in the Data Model. When the User joins a group call, the Call Engine retrieves or creates the Group Call from the registry. The Group Call establishes a media channel to the Telegram group call server infrastructure rather than directly to each participant. Participants are listed with their video endpoint identifiers, and the Group Call subscribes to the video streams of the visible participants. The User may raise a hand, mute others if permitted, enable screen sharing, and view participant lists through the Group Call interface.

#### 3.7.5 Call Termination and Rating

When either party ends a call, the One-to-One Call sends a discard message through the API Facade with a reason code. The Media Controller tears down all media streams. The Call Engine destroys the call object. After termination, the system optionally presents a rating prompt to the User. The User's rating is sent to the server through the API Facade. The Call Panel associated with the call is closed.


### 3.8 Notifications

#### 3.8.1 Notification Center Role

The Notification Center is a component that spans all accounts in the Domain Container. It subscribes to change events from the Change Notifier of each active Session. When a new message event is received, the Notification Center evaluates whether a notification should be displayed.

#### 3.8.2 Mute and Quiet-Hours Evaluation

Before formatting or displaying a notification, the Notification Center checks the following rules in order. First, it checks whether the global notification mute is active. Second, it checks whether the originating conversation has a per-peer mute applied. Third, it checks whether the current time falls within the User's configured quiet-hours window. Fourth, it checks whether the message type is excluded by the User's notification skip rules. If any check suppresses the notification, the Notification Center discards the event without displaying anything.

#### 3.8.3 Notification Formatting

For notifications that pass all mute checks, the Notification Center formats the notification title and body text. The title contains the sender or conversation name. The body contains a message preview, unless the User has disabled message previews in notification settings or the message belongs to a secret chat, in which case the body contains a generic placeholder. Custom notification sounds are selected by matching the peer's configured sound setting.

#### 3.8.4 Platform Delegation

The Notification Center passes the formatted notification to the Notification Backend. The Notification Backend is a platform-specific component that calls the operating system notification API to display the notification natively. On platforms where a native notification API is not available or is disabled, the Notification Backend presents an in-application notification overlay. The Notification Backend also handles notification click events and forwards them to the Session Controller to navigate to the relevant conversation.


### 3.9 Local Storage and Encryption

#### 3.9.1 Domain Storage and Local Key

Domain Storage holds the account index and the Local Key for the installation. When the application starts, the Domain Storage is read to enumerate configured accounts and to retrieve the Local Key. If a passcode is set, the Local Key is encrypted under the passcode-derived key and cannot be decrypted without the correct passcode. When no passcode is set, the Local Key is stored in a protected but non-passcode-encrypted form.

The Local Key is the symmetric key used to encrypt all account-level local data. No account data is written to disk in unencrypted form.

#### 3.9.2 Account Storage Layout

Account Storage is the per-account local store. It holds authentication tokens, session data, cached peer and message data, message drafts, session settings, sticker set information, the File Location Map, and trusted-peer flags. All records in Account Storage are encrypted with the Local Key before being written to disk. Account Storage uses a partitioned file layout that separates authentication data from cache data, allowing the cache to be cleared without affecting authentication.

#### 3.9.3 Settings Store

The Settings Store holds application-level preferences that are not tied to any specific account: application theme, language pack selection, background image, and global audio settings. The Settings Store is shared across all accounts. Its contents are not encrypted with the per-account Local Key but may be protected by the platform file system.

#### 3.9.4 File Location Map

The File Location Map is a record within Account Storage that maps media keys, which are derived from server-assigned file identifiers, to on-disk file paths. When a downloaded media file has been saved to the User's file system at their request, the File Location Map records the path. On subsequent requests for the same media, the File Loader consults the File Location Map before querying the Local Cache Database or making network requests.

#### 3.9.5 Passcode Lock and Key Derivation

When the User enables the local passcode, the Application Core re-derives the Local Key from the new passcode, re-encrypts all Account Storage with the new Local Key, and stores the encrypted key in Domain Storage. When the User locks the application, the Application Core discards the decrypted Local Key from memory. All further access to local account data is blocked until the passcode is entered. On correct passcode entry, the Local Key is re-derived, decrypted, and restored to memory, and the application unlocks.


### 3.10 UI Navigation and Window Management

#### 3.10.1 Window Controller and Session Controller

The Window Controller is the per-window coordinator. It binds a single Main Window to a single Account. The Window Controller creates a Session Controller when an Account has an active Session. The Session Controller is the navigation coordinator for the window: it maintains the current section, the active Peer, and the layer stack, and routes all navigation requests within its window.

#### 3.10.2 Main Content Widget Layout

The Main Content Widget hosted inside the Main Window presents a two-column or three-column layout. The left column contains the Dialogs List. The center column contains the active conversation or content section. An optional right column holds supplementary information such as a user profile or group member list. The Session Controller controls which section occupies each column and drives slide animations when the layout changes.

The Dialogs List is backed by the Data Model's dialog entries and is updated in response to Change Notifier events. Filtering and folder navigation are managed by the Session Controller, which instructs the Dialogs List to display the appropriate subset of conversations.

#### 3.10.3 Layer Stack

The Session Controller maintains a layer stack above the Main Content Widget for modals and overlays such as settings panels, image viewers, and confirmation dialogs. Layers are pushed onto the stack when opened and popped when closed. The topmost layer receives input events and obscures layers beneath it. Navigation requests that would change the underlying content section while a layer is open queue behind the layer.

#### 3.10.4 Intro Flow Window Management

Before a Session is established, the Window Controller hosts the Intro Flow instead of the Session Controller. The Intro Flow occupies the Main Window and presents the sequence of authentication Intro Steps. On successful authentication, the Intro Flow is replaced by the Session Controller, which initialises the main content view.

#### 3.10.5 Multi-Window and Detached Chat Layouts

The Session Controller supports opening a conversation in a detached window. A detached chat window is managed by a secondary Window Controller linked to the same Session Controller as the primary window. Navigation events in the primary window do not affect detached windows. The Session Controller tracks all open windows for a session and coordinates their lifecycle.

When multiple accounts are configured, each Account may have its own Window Controller if multi-window mode is active. The Domain Container manages the set of open windows and ensures that account switching brings the correct window to focus.

#### 3.10.6 Call Panel Integration

When an active call exists, the Call Panel is displayed as a detached window managed by the Call Engine. The Call Panel presents call controls, the Key Fingerprint emoji display, and video streams if applicable. The Call Panel is independent of the Session Controller window layout and persists until the call is terminated. The Call Engine notifies the Application Core when a call ends, and the Application Core closes the Call Panel.
