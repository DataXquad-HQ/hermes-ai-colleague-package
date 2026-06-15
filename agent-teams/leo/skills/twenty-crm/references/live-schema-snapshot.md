# Twenty CRM — Live Schema Snapshot
*Verified: 2026-06-14 via GraphQL introspection against localhost:3001*

## GraphQL Endpoints (confirmed working)
- `POST /graphql` — data CRUD ✅
- `POST /metadata` — schema introspection ✅
- `POST /api` — **404, does not exist** ❌

## Object Counts (at time of snapshot)
- People: 16
- Companies: 18
- Opportunities: 14
- Partnerships: 9
- Tasks: 0
- Notes: 0

## Opportunity Stage Enum (OpportunityStageEnum)
`NEW` → `SCREENING` → `MEETING` → `PROPOSAL` → `CUSTOMER`

## Key Custom Enums

### OpportunityBusinessLineEnum
`BUSYCOW` | `GEOKERNEL` | `AQUAOPTIMA` | `TRACI` | `DISTIFY` | `DATAXQUAD`

### OpportunityDealTypeEnum
`DIRECT` | `PARTNERLED`

### OpportunityHealthCheckEnum
`ON_TRACK` | `NEEDS_FOLLOWUP` | `AWAITING_RESPONSE` | `AT_RISK`

### OpportunityPriorityEnum
`HIGH` | `MEDIUM` | `LOW`

### PersonStatusEnum
Check live schema — not fully introspected

### PersonCountryEnum
`TAIWAN` | `HONG_KONG` | `CHINA` | `MALAYSIA` | `THAILAND` | `INDONESIA` | `JAPAN` (from schema doc)

### PersonSourceEnum
`REFERRAL` | `EVENT` | `PARTNER` | `NETWORK` | `INBOUND_WEB` | `OUTBOUND_MAYA`

### PersonDecisionRoleEnum
`BUYER` | `USER` | `INFLUENCER` | `BLOCKER` | `CHAMPION`

### PersonPreferredChannelEnum
`EMAIL` | `WHATSAPP` | `LINE` | `PHONE`

## Opportunity Custom Fields (confirmed via introspection)
```
businessLine          OpportunityBusinessLineEnum
dealType              OpportunityDealTypeEnum
healthCheck           OpportunityHealthCheckEnum
priority              OpportunityPriorityEnum
nextFollowUpDate      DateTime
nextActionSummary     String
currentStatusSummary  String
overview              String
probability           Float
expectedValue         Currency
dealId                String
docLink               Links
primaryContact        String   (text annotation, not relation)
relevantContacts      Person   (relation)
otherContacts         Person   (relation)
```

## Partnership Custom Fields (confirmed via introspection)
```
stage                 PartnershipStageEnum
status                PartnershipStatusEnum
partnerType           PartnershipPartnerTypeEnum
startDate             DateTime
endDate               DateTime
lastUpdateDate        DateTime
currentStatusSummary  String
nextActionSummary     String
partnershipOverview   String
docLink               Links
primaryContact        Person   (M:1 relation)
relatedPeople         Person   (1:M relation)
company               Company  (M:1 relation)
```

## Person Custom Fields (confirmed via introspection)
```
status               PersonStatusEnum
country              PersonCountryEnum
decisionRole         PersonDecisionRoleEnum
source               PersonSourceEnum
preferredChannel     PersonPreferredChannelEnum
department           String
notes                String
remarks              String
lastContactDate      DateTime
relatedPartnerships  Partnership  (M:1)
primaryPartnerships  Partnership  (1:M)
engagementsAttended  Engagement
involvingOpportunities Opportunity
relevantDeals        Opportunity
```

## Schema Divergence from Docs (fixed 2026-06-14)
The `structural-data/crm/schema` page in Hindsight previously had these errors — now corrected:
- Opportunity stages were `D1/D2/D3/D4/S1/S2/CLOSED_WON/CLOSED_LOST` (old/wrong)
- `healthCheck` had `AWAITING` — actual is `AWAITING_RESPONSE`
- `dealType` had `PARTNERSHIP` and `INVESTMENT` options — actual is `PARTNERLED`
- `businessLine` field was missing entirely
- `priority` had `VERY_HIGH` option — actual is just `HIGH/MEDIUM/LOW`
