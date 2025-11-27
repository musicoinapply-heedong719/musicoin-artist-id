# Musicoin 3.0 – Artist ID (musicoin-artist-id)

## Overview

The **Musicoin Artist ID** system provides a unified, verifiable on-chain identity layer  
for all artists in the Musicoin 3.0 ecosystem.

It ensures that every artist has:
- A unique, immutable Artist ID  
- A verified public profile  
- References that can be used across all Musicoin services

The Artist ID becomes the **single source of truth** for:
- Musicoin Player (artist metadata)
- Sampling Studio (sample authorship)
- Ticketing System (event organizers)
- Instrument Provenance (linking instruments to artists)
- Contract Proof System (document owners)
- Analytics Dashboard (creator metrics)

This project defines the smart contract, registry logic, indexing API,  
and front-end interface for managing artist identities.

---

## Core Goals

- Provide a trusted on-chain registry of verified artists  
- Allow each artist to control their identity through their wallet  
- Standardize artist metadata used throughout all Musicoin dApps  
- Prevent impersonation, duplication, or fraud  
- Make artist discovery easier and more reliable  

Artist ID is the **foundation layer** for Musicoin’s decentralized content economy.

---

## Key Features

### 1. **Unique On-Chain Artist ID**
- Each artist receives a unique numerical ID (e.g., ArtistID = 1042)
- The ID is permanent and linked to one primary wallet address
- Optional ability to add secondary addresses (verified via signature)

### 2. **Artist Profile Metadata**
On-chain or hybrid (on/off-chain) metadata:
- Artist/stage name
- Official homepage URL
- Twitter/X handle
- Profile image hash (IPFS)
- Short description/bio
- Genres / tags

### 3. **Verification Layer**
- Artists verify ownership by signing a message  
- Official Musicoin admins (optional) can grant a “verified” badge
- Prevents spoofing or fake accounts

### 4. **Cross-DApp Integration**
Used by:
- Musicoin Player → display correct artist info  
- Sampling Studio → identify sample creators  
- Ticketing → event organizer profiles  
- Instrument Provenance → link instruments to artists  
- Contract Proof System → link contracts to identities  

### 5. **Search & Discovery API**
Backend indexing service provides:
- Search by name, genre, tag  
- Social link lookup  
- Artist ranking (via analytics dashboard)

---

## Architecture

The system uses a **three-tier structure**:

---

### **1. Smart Contract Layer (Core Artist Registry)**

Stores essential data:
- `artistId → wallet`
- `artistId → metadataHash`
- `wallet → artistId` (reverse lookup)
- `isVerified` flag

Functions:
- `registerArtist(string metadataHash)`
- `updateMetadata(string metadataHash)`
- `linkAdditionalWallet(address wallet)`
- `verifyArtist(uint artistId)`
- `getArtist(uint artistId)`

Metadata (detailed profile) is stored off-chain (IPFS or Arweave),  
with only hashes stored on-chain.

---

### **2. Indexing Layer (Backend API)**

Processes:
- Artist registration events  
- Profile updates  
- Verification events  
- Social link changes  

Provides:
- REST API or GraphQL  
- Cached metadata  
- Integration endpoints for all other Musicoin dApps  

Example endpoints:
- `GET /artists/:id`
- `GET /artists/search?q=keyword`
- `GET /artists/:id/tracks`
- `GET /artists/:id/samples`
- `GET /artists/:id/events`

---

### **3. Front-End Layer (Artist Portal)**

Provides a simple UI for:
- Registering a new artist  
- Editing metadata  
- Managing social links  
- Uploading profile images  
- Viewing verification status  

Stack recommendation:
- React (Next.js) + Wagmi/Web3Modal  
- IPFS file uploader (Pinata / Web3.Storage)  
- TailwindCSS for quick styling

---

## Data Model

### On-chain Fields
| Field | Type | Description |
|------|------|-------------|
| artistId | uint256 | Unique incremental ID |
| wallet | address | Primary address of the artist |
| metadataHash | string | IPFS hash of profile metadata |
| isVerified | bool | Verified badge status |
| createdAt | uint | Block timestamp |
| updatedAt | uint | Last update timestamp |

---

### Off-chain Metadata (IPFS)
```json
{
  "name": "Artist Name",
  "homepage": "https://artist-site.com",
  "twitter": "https://twitter.com/artist",
  "bio": "Short introduction or description",
  "profileImage": "ipfs://Qm....",
  "genres": ["electronic", "jazz"],
  "tags": ["producer", "dj", "composer"]
}
