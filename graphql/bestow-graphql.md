# Bestow GraphQL Schema

## Overview

This conceptual GraphQL schema models the Bestow life insurance platform, which powers embedded insurance through the Bestow Protect API. Bestow provides fintechs, banks, financial institutions, P&C carriers, mortgage platforms, and benefits providers the ability to offer instant term life, final expense, and IUL coverage inside their own applications and websites.

The schema covers the full lifecycle of embedded life insurance: quoting, application, underwriting, policy issuance, beneficiary management, claims, and partner/agent integration.

## Schema Source

Conceptual schema derived from public Bestow documentation, partner API references at https://api.hellobestow.com, the Bestow developer portal at https://developers.bestow.com/, and publicly available information about the Bestow Protect API and embedded insurance platform.

## Types

### Application Domain

- **Application** — top-level application entity linking a quote to an applicant and tracking lifecycle state
- **ApplicationDetails** — extended metadata: submission timestamp, channel, partner reference ID, language preference
- **ApplicationStatus** — enumeration of application lifecycle states (STARTED, SUBMITTED, PENDING, APPROVED, DECLINED, REFERRED, CANCELLED)

### Quote Domain

- **Quote** — rate quote generated for an applicant based on age, health, coverage, and term inputs
- **QuoteDetails** — full breakdown of a quote including carrier, product type, and underwriting tier
- **QuoteTerms** — available term lengths and corresponding premium amounts for a given quote

### Policy Domain

- **Policy** — issued life insurance policy with policy number, effective date, and status
- **PolicyDetails** — extended policy data: lapse date, grace period, reinstatement eligibility, payment method
- **PolicyStatus** — enumeration of policy states (ACTIVE, LAPSED, CANCELLED, MATURED, CLAIMED, PENDING_ISSUE)
- **PolicyDocument** — digital document attached to a policy (policy contract, declarations page, welcome letter)

### Coverage Domain

- **Coverage** — coverage layer associated with a policy defining what is insured and for how much
- **CoverageDetails** — product-level detail: product name, coverage category, exclusion riders
- **CoverageAmount** — structured representation of the face amount with currency, minimum, and maximum bounds

### Term Domain

- **Term** — insurance term selected at quote/application time
- **TermDetails** — extended term data including start date, end date, and renewal options
- **TermLength** — enumeration of supported term lengths in years (10, 15, 20, 25, 30)

### Premium Domain

- **Premium** — premium record for a policy including amount, due date, and payment status
- **PremiumDetails** — modal premium breakdown: annual, semi-annual, quarterly, monthly equivalents
- **PremiumFrequency** — enumeration of billing frequencies (MONTHLY, QUARTERLY, SEMI_ANNUAL, ANNUAL)

### Beneficiary Domain

- **Beneficiary** — designated recipient of death benefit proceeds
- **BeneficiaryDetails** — relationship, date of birth, SSN last four, contact information
- **BeneficiaryType** — enumeration (PRIMARY, CONTINGENT, IRREVOCABLE)

### Applicant Domain

- **Applicant** — person applying for coverage including identity and contact fields
- **ApplicantDetails** — full applicant profile: address, employment, income, citizenship status
- **ApplicantDemographics** — age, gender, height, weight, tobacco use, build table score

### Health and Underwriting Domain

- **HealthQuestion** — single health question presented during application (text, category, required flag)
- **HealthAnswer** — applicant's response to a health question with any follow-up detail
- **RiskProfile** — aggregated risk assessment derived from health answers, demographics, and data vendor outputs
- **UnderwritingDetails** — full underwriting record: decision date, tier, sub-standard ratings, exclusions
- **UnderwritingDecision** — enumeration of underwriting outcomes (APPROVED, DECLINED, REFERRED, POSTPONED, TABLE_RATED)
- **ReferralDetails** — referral case metadata: referral reason, assigned underwriter, required evidence items
- **DeathBenefit** — configured death benefit amount and any accelerated benefit riders attached

### Offer Domain

- **Offer** — underwriting offer extended to an applicant post-decision
- **OfferDetails** — offer terms: coverage amount, premium, effective date, acceptance deadline
- **OfferExpiry** — expiration metadata for an outstanding offer
- **IssueDetails** — policy issuance record: issue date, delivery method, e-delivery confirmation

### Claim Domain

- **Claim** — death claim filed against an active policy
- **ClaimDetails** — claim intake data: claimant name, relationship to insured, date of death, cause of death
- **ClaimStatus** — enumeration of claim states (FILED, UNDER_REVIEW, APPROVED, PAID, DENIED, PENDING_DOCUMENTS)
- **BenefitPayment** — record of benefit payment disbursed to a beneficiary including amount, date, and payment method

### Agent and Partner Domain

- **Agent** — licensed agent associated with a policy or application
- **AgentDetails** — agent license number, resident state, NPN, appointed carriers
- **Carrier** — insurance carrier underwriting policies on the Bestow platform
- **CarrierDetails** — carrier NAIC code, state approvals, product lines, AM Best rating
- **PartnerDetails** — embedded partner configuration: partner ID, branding, allowed products, API access tier
- **Integration** — partner integration record: integration type, status, connected systems

### Auth and Webhook Domain

- **APIKey** — API key credential used to authenticate Protect API requests
- **Token** — short-lived OAuth access token for API sessions
- **Webhook** — webhook endpoint registered by a partner to receive event notifications
- **WebhookEvent** — individual event payload delivered to a partner webhook endpoint

## Queries

- `getQuote(input: QuoteInput!)` — retrieve a life insurance quote
- `getApplication(id: ID!)` — fetch an application by ID
- `getPolicy(id: ID!)` — fetch a policy by ID
- `listPolicies(partnerId: ID!, status: PolicyStatus)` — list policies for a partner
- `getApplicant(id: ID!)` — fetch applicant profile
- `listHealthQuestions(productType: String!)` — list health questions for a product
- `getUnderwritingDetails(applicationId: ID!)` — retrieve underwriting record for an application
- `getOffer(applicationId: ID!)` — retrieve outstanding offer for an application
- `getClaim(id: ID!)` — fetch a claim by ID
- `listWebhooks(partnerId: ID!)` — list registered webhooks for a partner
- `getCarrier(id: ID!)` — fetch carrier details

## Mutations

- `startApplication(input: ApplicationInput!)` — initiate a new application
- `submitApplication(id: ID!)` — submit a completed application for underwriting
- `acceptOffer(offerId: ID!)` — accept an underwriting offer to issue a policy
- `declineOffer(offerId: ID!)` — decline an underwriting offer
- `addBeneficiary(policyId: ID!, input: BeneficiaryInput!)` — add a beneficiary to a policy
- `updateBeneficiary(id: ID!, input: BeneficiaryInput!)` — update beneficiary details
- `removeBeneficiary(id: ID!)` — remove a beneficiary designation
- `fileClaim(policyId: ID!, input: ClaimInput!)` — file a death claim
- `registerWebhook(input: WebhookInput!)` — register a partner webhook endpoint
- `deactivateWebhook(id: ID!)` — deactivate a webhook endpoint
- `generateAPIKey(partnerId: ID!)` — generate a new API key for a partner

## Notes

This schema is conceptual and intended for modeling purposes. The Bestow Protect API is a partner-gated REST API; GraphQL is not confirmed as a supported interface. The schema reflects the domain objects described in public Bestow product documentation and partner API surface descriptions.
