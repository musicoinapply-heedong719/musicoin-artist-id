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
```

---

## Registration Flow

### 1. Wallet Connect  
Artist connects their wallet through the portal.

### 2. Metadata Upload  
Basic profile is uploaded to IPFS → returns metadata hash.

### 3. Contract Call  
Artist calls:
```
registerArtist(metadataHash)
```

### 4. Artist ID Issued  
A unique ID is generated (auto-increment).

### 5. Verification (Optional)  
Admin or algorithm confirms:
- Social links  
- Official channels  
- Identity consistency  

Artist receives ✔ Verified Badge.

---

## Integration With Other Projects

| Musicoin Project | Usage of Artist ID |
|------------------|--------------------|
| **musicoin-player** | Displays profile + homepage/Twitter |
| **musicoin-sampling-studio** | Sample authorship tracking |
| **musicoin-ticketing** | Event organizer identity |
| **instrument-history** | Artist → instrument ownership link |
| **contract-proof** | Legal contracts tied to correct identity |
| **analytics-dashboard** | Revenue, plays, growth tracking |

Artist ID acts as the **root identity connector** for the entire ecosystem.

---

## Installation

### Clone
```bash
git clone https://github.com/musicoin/musicoin-artist-id.git
cd musicoin-artist-id
```

### Smart Contract Setup
```bash
cd contracts
npm install
```

Deploy (example):
```bash
npx hardhat run scripts/deploy.js --network polygon
```

### Backend API Setup
```bash
cd backend
npm install
npm run dev
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

---

## Roadmap

### ✔ Phase 1 – Core Registry
- Artist ID issuance  
- Metadata hash storage  
- Basic portal UI  

### ✔ Phase 2 – Verification & Social Linking
- Verified badge  
- Social media validation  
- Profile image support  

### ✔ Phase 3 – Cross-Ecosystem Integration
- Player integration  
- Ticketing integration  
- Sampling studio authorship linkage  

### ✔ Phase 4 – Analytics + Advanced Metadata
- Artist growth statistics  
- Advanced tags (DAW, equipment, style)  
- Multi-language bios  

---

## Contribution

Contributions are welcome in all areas:
- Smart contract design  
- Frontend portal UX  
- Indexer logic  
- API extensions  
- Security and verification improvements  

Please submit issues and pull requests.

---

## License

TBD  
(MIT or Apache-2.0 recommended)
