# Acceptance Criteria: OmniPitch

## Document Information

**Project Name:** OmniPitch  
**Document Type:** Acceptance Criteria & Definition of Done  
**Version:** 1.0  
**Date Created:** September 1, 2026  
**Status:** Active

---

## 1. Introduction

This document defines acceptance criteria for core features of the OmniPitch streaming platform. Each feature is specified through user stories using the Gherkin language (Given-When-Then format) to ensure clear, testable requirements that all stakeholders can understand.

---

## 2. User Story: User Registration

### 2.1 Story

**As a** new football fan  
**I want to** create an OmniPitch account  
**So that** I can access the streaming platform and manage my preferences

### 2.2 Acceptance Criteria

#### AC 2.2.1: Valid Email Registration

**Given** I am on the OmniPitch registration page  
**When** I enter:
- Email: "john.doe@example.com"
- Password: "SecurePass123!"
- Full Name: "John Doe"
- Preferred Language: "English"
**And** I click the "Create Account" button  
**Then** the system should:
- Accept my registration
- Display a success message: "Registration successful! Please verify your email."
- Send a verification email to "john.doe@example.com"
- Redirect me to the email verification page
- Store my account data in the database

---

#### AC 2.2.2: Email Verification

**Given** I have registered with email "jane.smith@example.com"  
**And** I receive a verification email with a verification link  
**When** I click the verification link within 24 hours  
**Then** the system should:
- Mark my email as verified
- Display confirmation message: "Email verified successfully!"
- Activate my account for login
- Redirect me to the login page
- Send a welcome email with platform introduction

---

#### AC 2.2.3: Email Verification Expiration

**Given** I registered but did not verify my email  
**And** 24 hours have passed since registration  
**When** I attempt to click the verification link  
**Then** the system should:
- Display error message: "Verification link has expired"
- Offer option to "Resend Verification Email"
- Allow me to request a new verification link
- Send a fresh verification email within 1 minute

---

#### AC 2.2.4: Duplicate Account Prevention

**Given** an account already exists with email "alice@example.com"  
**When** I attempt to register with the same email address  
**And** I click "Create Account"  
**Then** the system should:
- Reject the registration
- Display error message: "An account with this email already exists"
- Offer option to "Login" or "Reset Password"
- NOT create a new account

---

#### AC 2.2.5: Password Complexity Validation

**Given** I am on the registration page  
**When** I enter password "weak"  
**Then** the system should:
- Display error: "Password must be at least 8 characters"

**When** I enter password "NoDigits!"  
**Then** the system should:
- Display error: "Password must contain at least one digit"

**When** I enter password "nouppercasehere1"  
**Then** the system should:
- Display error: "Password must contain at least one uppercase letter"

**When** I enter password "NoSpecial123"  
**Then** the system should:
- Display error: "Password must contain at least one special character (!@#$%^&*)"

**When** I enter password "ValidPass123!"  
**Then** the system should:
- Accept the password
- Enable the "Create Account" button

---

#### AC 2.2.6: Required Field Validation

**Given** I am on the registration page  
**When** I leave the email field empty  
**And** I click "Create Account"  
**Then** the system should:
- Display error: "Email is required"
- Highlight the email field in red
- NOT submit the form

**When** I leave the name field empty  
**And** I click "Create Account"  
**Then** the system should:
- Display error: "Full name is required"
- Highlight the name field in red

**When** I leave the preferred language unselected  
**And** I click "Create Account"  
**Then** the system should:
- Set default language to "English"
- Display info message: "Language set to English"
- Continue with registration

---

#### AC 2.2.7: Terms of Service Acceptance

**Given** I am on the registration page  
**When** I complete all registration fields  
**And** I do NOT check the "I agree to Terms of Service" checkbox  
**And** I click "Create Account"  
**Then** the system should:
- Display error: "You must agree to the Terms of Service"
- Highlight the checkbox in red
- NOT create the account

**When** I check the checkbox  
**And** I click "Create Account"  
**Then** the system should:
- Accept the terms acceptance
- Proceed with account creation

---

#### AC 2.2.8: Registration Data Persistence

**Given** I have successfully registered  
**When** I log in with my credentials  
**Then** the system should:
- Retrieve and display all my registration data correctly
- Show my full name: "John Doe"
- Show my preferred language: "English"
- Show my email: "john.doe@example.com"
- Maintain all preferences as entered during registration

---

## 3. User Story: Select Streaming Quality

### 3.1 Story

**As a** subscriber watching a football match  
**I want to** select the video quality (resolution and bitrate)  
**So that** I can optimize streaming based on my device and internet connection

### 3.2 Acceptance Criteria

#### AC 3.2.1: Quality Options Display

**Given** I am watching a live match stream  
**When** I open the video player  
**Then** the system should:
- Display a "Settings" or "Quality" menu button in the player controls
- Make the button easily accessible (bottom right corner of player)
- Display button label: "⚙️ Settings" or "⚙️"

---

#### AC 3.2.2: Quality Selection Menu

**Given** I am watching a match  
**When** I click the "Settings" button  
**Then** the system should:
- Display a dropdown menu with quality options:
  - Auto (default) - Recommended
  - 360p (1 Mbps)
  - 720p (3 Mbps)
  - 1080p (5 Mbps)
  - 4K (15 Mbps)
- Display current selection with a checkmark
- Display estimated bitrate for each option

---

#### AC 3.2.3: Auto Quality Selection

**Given** quality is set to "Auto"  
**When** my network bandwidth is 2 Mbps  
**Then** the system should:
- Automatically select 360p quality
- Stream video at 360p resolution

**When** my network bandwidth increases to 4 Mbps  
**Then** the system should:
- Automatically adjust to 720p quality within 5 seconds
- Maintain smooth playback without interruption

**When** my network bandwidth decreases to 1.5 Mbps  
**Then** the system should:
- Automatically drop to 360p quality
- Prevent buffering and rebuffering

---

#### AC 3.2.4: Manual Quality Selection - 720p

**Given** I am watching a match  
**When** I click "Settings" > "720p"  
**Then** the system should:
- Switch to 720p quality
- Display checkmark next to "720p"
- Maintain smooth playback during transition
- Display "720p" in video player quality indicator
- Complete transition within 3 seconds
- Load next video segment in 720p resolution

---

#### AC 3.2.5: Manual Quality Selection - 1080p

**Given** I am watching a match  
**When** I click "Settings" > "1080p"  
**Then** the system should:
- Switch to 1080p quality
- Confirm bitrate requirement of 5 Mbps is available
- Display warning if bandwidth insufficient: "Your connection may not support 1080p. Buffering may occur."
- Switch quality if I confirm
- Display "1080p" in quality indicator

---

#### AC 3.2.6: Manual Quality Selection - 4K

**Given** I am watching a match  
**And** my subscription tier is "Pro" (4K access)  
**When** I click "Settings" > "4K"  
**Then** the system should:
- Check if device supports 4K (compatible display)
- Check if bandwidth ≥ 15 Mbps
- Switch to 4K quality
- Stream match in 4K resolution
- Display "4K" in quality indicator

**Given** my subscription tier is "Premium" (limited to 1080p)  
**When** I try to select "4K"  
**Then** the system should:
- Display message: "4K streaming is available only with Pro subscription"
- Offer upgrade option
- NOT enable 4K streaming

---

#### AC 3.2.7: Quality Preference Persistence

**Given** I am logged into my account  
**When** I select "1080p" and watch a match  
**And** I close the match  
**And** I start watching another match 1 hour later  
**Then** the system should:
- Remember my quality preference as "1080p"
- Automatically stream the new match in 1080p
- Display "1080p" as current quality selection

---

#### AC 3.2.8: Quality Switching During Stream

**Given** I am streaming a match in 720p  
**And** the match is currently at 45th minute (not paused)  
**When** I click "Settings" > "1080p"  
**Then** the system should:
- Switch quality without stopping playback
- Maintain video playback continuity
- Buffer next segment in 1080p within 2 seconds
- Display "1080p" in quality indicator
- NOT skip content or create noticeable gaps

---

#### AC 3.2.9: Low Bandwidth Warning

**Given** I have selected "1080p" quality  
**And** my network bandwidth drops to 3 Mbps (insufficient for 1080p)  
**When** the system detects insufficient bandwidth  
**Then** the system should:
- Display warning banner: "Network conditions have changed. Reducing quality to prevent buffering."
- Automatically drop to 720p
- Wait 5 seconds before switching to allow temporary recovery
- Resume playback in reduced quality

---

#### AC 3.2.10: Quality Indicator Display

**Given** I am watching a match  
**When** I do not interact with the player for 5 seconds  
**Then** the system should:
- Hide player controls
- Display only a small quality indicator (e.g., "1080p" in bottom right)

**When** I move my mouse over the player  
**Then** the system should:
- Display all player controls
- Show quality indicator in the Settings menu

---

## 4. User Story: Switch Commentary Audio Languages During Live Match

### 4.1 Story

**As a** multilingual football fan  
**I want to** switch between different language commentary tracks during a live match  
**So that** I can enjoy the match in my preferred language without interruption

### 4.2 Acceptance Criteria

#### AC 4.2.1: Available Languages Display

**Given** I am watching a live Premier League match  
**When** I open the video player  
**And** I click the "Audio" or "Language" button  
**Then** the system should:
- Display list of available commentary languages:
  - English (Mark Lawrenson)
  - Spanish (Jorge Valdano)
  - German (Marcel Reif)
  - Italian (Pierluigi Pardo)
  - French (Thierry Henry)
  - Portuguese (Paulo Futre)
  - Dutch (Rafael Van der Vaart)
  - Polish (Dariusz Szpakowski)
- Display current selected language with checkmark
- Display commentator name next to language
- Display quality of audio (Stereo/Mono)

---

#### AC 4.2.2: Switch to Spanish Commentary

**Given** I am watching a live match with English commentary  
**When** I click "Audio/Language"  
**And** I select "Spanish (Jorge Valdano)"  
**Then** the system should:
- Switch audio track to Spanish within 1 second
- Display "Spanish" as current language
- Continue match playback without interruption
- NOT skip any content or events during switch
- Display visual feedback: "Switched to Spanish commentary"

---

#### AC 4.2.3: Instant Audio Switch During Live Action

**Given** I am watching a live match at minute 72 with English commentary  
**When** a goal is scored at minute 72:30  
**And** I immediately click "Audio" > "German"  
**Then** the system should:
- Switch to German commentary within 0.5 seconds
- Play German commentator's reaction to the goal
- NOT miss the goal announcement in German
- Display "German" as current language
- Sync German audio to current match time (72:30)

---

#### AC 4.2.4: Audio Track Persistence

**Given** I am logged into my account  
**When** I select "Spanish" commentary for a match  
**And** I watch the match for 30 minutes  
**And** I close the stream  
**And** I start watching another match 2 hours later  
**Then** the system should:
- Remember "Spanish" as my preferred commentary language
- Automatically load Spanish commentary for the new match
- Display "Spanish" in the audio selector
- Load Spanish audio track before starting playback

---

#### AC 4.2.5: Default Language Selection

**Given** I have set my profile preferred language to "Portuguese"  
**When** I start watching a new match  
**And** Portuguese commentary is available  
**Then** the system should:
- Default to Portuguese commentary
- Automatically load Portuguese audio
- Display "Portuguese" as selected language

**When** Portuguese commentary is NOT available for a match  
**Then** the system should:
- Fall back to English commentary
- Display message: "Portuguese commentary unavailable. Switched to English."

---

#### AC 4.2.6: Audio Quality Indicator

**Given** I am watching a match  
**When** I click "Audio/Language"  
**Then** the system should:
- Display audio quality next to each language:
  - English: Stereo 192 kbps
  - Spanish: Stereo 192 kbps
  - German: Stereo 192 kbps
  - Italian: Mono 128 kbps (lower quality)

---

#### AC 4.2.7: Unavailable Language Handling

**Given** I am watching a Bundesliga match  
**When** I open the audio language selector  
**And** 8 languages are available but Greek is NOT available  
**Then** the system should:
- Display only available languages
- Display Greek language as "UNAVAILABLE" with gray-out effect
- Display message: "Greek commentary not available for this match"
- NOT allow selection of unavailable language

---

#### AC 4.2.8: Audio Sync with Video

**Given** I am watching a live match  
**When** I switch from English to German commentary at minute 45:30  
**Then** the system should:
- Sync German audio to current match time (45:30)
- Display German commentary starting from exact same moment
- NO audio/video sync delay (max 100ms)
- German audio matches video content shown on screen

---

#### AC 4.2.9: Mobile Audio Language Switching

**Given** I am watching a match on mobile (iOS/Android)  
**When** I tap the "Audio" icon in the player  
**Then** the system should:
- Display audio language selector overlay
- Display landscape orientation optimization
- Show 5 most recent languages at top for quick access
- Display full language list with scroll if needed

**When** I select "Italian"  
**Then** the system should:
- Close selector overlay
- Switch to Italian audio within 1 second
- Display language indicator "ITA" in top right

---

#### AC 4.2.10: Subtitle Sync with Audio Language

**Given** I am watching a match in English with English subtitles  
**When** I switch audio to Spanish  
**And** I have "Auto-match subtitle to audio" enabled in settings  
**Then** the system should:
- Automatically switch subtitles to Spanish (if available)
- Sync Spanish subtitles with Spanish commentary
- Display Spanish subtitles matching Spanish dialog

---

#### AC 4.2.11: Audio Language in On-Demand Replays

**Given** I am watching a recorded match from yesterday  
**When** I open the audio language selector  
**Then** the system should:
- Display all available commentary languages for that match
- Allow switching between languages same as live match
- Sync audio to current playback position
- Remember language selection for future replays of same match

---

#### AC 4.2.12: Multiple Device Consistency

**Given** I watched a match on my laptop in Spanish commentary  
**When** I resume the match on my mobile phone 10 minutes later  
**Then** the system should:
- Automatically load Spanish commentary
- Display at same playback position
- Maintain audio language preference across devices

---

## 5. User Story: Manage Subscription Plan

### 5.1 Story

**As a** OmniPitch user  
**I want to** manage my subscription plan (upgrade, downgrade, cancel, pause)  
**So that** I can control my costs and access to features based on my needs

### 5.2 Acceptance Criteria

#### AC 5.2.1: Subscription Status Display

**Given** I am logged into my OmniPitch account  
**When** I navigate to Account Settings > Subscription  
**Then** the system should:
- Display current subscription tier: "Premium" or "Pro" or "Free"
- Display next billing date: "October 15, 2026"
- Display billing frequency: "Monthly" or "Annual"
- Display subscription price: "€9.99/month" or "€99.99/year"
- Display renewal status: "Active" (green) or "Cancelled" (red)
- Display option to "Manage Subscription"
- Display "Upgrade to Pro" if user not on highest tier

---

#### AC 5.2.2: View Available Plans

**Given** I am on the Subscription management page  
**When** I click "View All Plans"  
**Then** the system should:
- Display 3 subscription tiers in clear comparison table:

| Feature | Free | Premium | Pro |
|---------|------|---------|-----|
| Price | €0/month | €9.99/month | €19.99/month |
| Max Quality | 720p | 1080p | 4K |
| Concurrent Streams | 1 | 2 | 4 |
| Matches/Month | 5 | Unlimited | Unlimited |
| Languages | English | All | All |
| Ads | Yes | No | No |

- Display "Current Plan" badge on my current tier
- Display "Upgrade" button on higher tiers
- Display "Downgrade" button on lower tiers (if applicable)

---

#### AC 5.2.3: Upgrade from Free to Premium

**Given** I have a Free tier subscription  
**When** I click "Upgrade to Premium"  
**Then** the system should:
- Display Premium plan details
- Show pricing: "€9.99/month"
- Display features included in Premium
- Show comparison with current Free plan
- Display "Select Premium" button

**When** I click "Select Premium"  
**Then** the system should:
- Redirect to checkout page
- Show billing summary: "Premium Subscription - €9.99/month"
- Display payment method options (Credit Card, PayPal, Apple Pay, Google Pay)
- Show current payment method (if exists)

---

#### AC 5.2.4: Payment Processing for Upgrade

**Given** I am on the checkout page for Premium upgrade  
**When** I select payment method "Credit Card"  
**And** I enter valid card details:
- Card: 4111 1111 1111 1111
- Expiry: 12/28
- CVV: 123
**And** I click "Complete Purchase"  
**Then** the system should:
- Process payment via Stripe
- Display "Processing..." indicator
- Show confirmation message: "Payment successful!"
- Display order confirmation number: ORD-20260901-12345
- Send confirmation email to user
- Send receipt email with invoice
- Update subscription to "Premium" tier

---

#### AC 5.2.5: Payment Failure Handling

**Given** I am on the checkout page  
**When** I enter invalid card details  
**And** I click "Complete Purchase"  
**Then** the system should:
- Attempt payment processing
- Receive decline response from payment processor
- Display error message: "Payment failed. Please check your card details."
- Highlight card fields in red
- NOT charge user
- NOT upgrade subscription
- Display support contact information

---

#### AC 5.2.6: Upgrade from Premium to Pro

**Given** I have Premium subscription (€9.99/month)  
**When** I click "Upgrade to Pro"  
**Then** the system should:
- Display Pro plan details
- Calculate upgrade cost: "€9.99 difference (€19.99 - €9.99)"
- Display message: "Upgrade cost: €9.99. Billing date: Oct 15"
- Display features unlock on upgrade: 4K quality, 4 concurrent streams
- Display "Confirm Upgrade" button

**When** I click "Confirm Upgrade"  
**Then** the system should:
- Process pro-rated payment: €9.99
- Update subscription to "Pro"
- Display confirmation: "Successfully upgraded to Pro!"
- Send confirmation email
- Unlock Pro features immediately
- NEW renewal date: November 15, 2026

---

#### AC 5.2.7: Downgrade Subscription

**Given** I have Pro subscription (€19.99/month)  
**And** next billing date is October 15, 2026  
**When** I click "Downgrade"  
**Then** the system should:
- Display available downgrade options: Premium (€9.99) or Free (€0)
- Display warning: "You will lose access to 4K streaming and 4 concurrent streams"
- Show refund calculation if applicable
- Display "Confirm Downgrade" button

**When** I select Premium and click "Confirm Downgrade"  
**Then** the system should:
- Process downgrade to Premium
- Calculate pro-rated refund: €X.XX (credited to next billing)
- Display confirmation: "Downgraded to Premium. Next billing: Oct 15 at €9.99"
- Send downgrade confirmation email
- Restrict 4K streaming access immediately
- Limit concurrent streams to 2

---

#### AC 5.2.8: Cancel Subscription

**Given** I have an active Premium subscription  
**When** I click "Cancel Subscription"  
**Then** the system should:
- Display cancellation confirmation dialog
- Show message: "Are you sure? You will lose access to Premium features on Oct 15, 2026"
- Display "Cancel Subscription" and "Keep Subscription" buttons
- Display reason for cancellation dropdown:
  - Too expensive
  - Not enough content
  - Technical issues
  - Other

**When** I select reason and click "Cancel Subscription"  
**Then** the system should:
- Mark subscription as "Cancelled"
- Display confirmation: "Your subscription will end on October 15, 2026"
- Show: "Access remains until cancellation date"
- Send cancellation confirmation email
- Display retention offer: "Come back later and get 50% off first month"

---

#### AC 5.2.9: Pause Subscription

**Given** I have an active Premium subscription  
**When** I click "Pause Subscription"  
**Then** the system should:
- Display pause options:
  - Pause for 1 month
  - Pause for 2 months
  - Pause for 3 months
- Display message: "You won't be charged during pause period"
- Display "Confirm Pause" button

**When** I select "Pause for 1 month" and click "Confirm Pause"  
**Then** the system should:
- Change status to "Paused"
- Revoke access to Premium features
- Downgrade to Free tier during pause
- Display "Resume" button instead of "Pause"
- Send pause confirmation email
- Schedule automatic resume for November 15, 2026

---

#### AC 5.2.10: Resume Paused Subscription

**Given** I have a paused subscription  
**When** I click "Resume Subscription"  
**Then** the system should:
- Display confirmation: "Resume Premium subscription?"
- Show resumption date and pricing
- Display "Confirm Resume" button

**When** I click "Confirm Resume"  
**Then** the system should:
- Restore subscription to "Active" status
- Restore Premium features and access
- Charge next billing amount on resumption date
- Send resumption confirmation email
- Display "Subscription Active" status

---

#### AC 5.2.11: Billing History

**Given** I am on my Subscription management page  
**When** I click "View Billing History"  
**Then** the system should:
- Display list of all past transactions:
  - Date: Sept 15, 2026
  - Description: Premium Subscription (Monthly)
  - Amount: €9.99
  - Status: Completed ✓
  - Invoice: [Download PDF]
- Sort by date (newest first)
- Allow filtering by date range
- Display total amount paid year-to-date

---

#### AC 5.2.12: Change Billing Cycle

**Given** I have Premium subscription on monthly billing (€9.99/month)  
**When** I click "Change Billing Cycle"  
**Then** the system should:
- Display billing options:
  - Monthly: €9.99/month
  - Annual: €99.99/year (16.7% discount)
- Show savings calculation: "Save €19.89/year with annual billing"
- Display "Select Annual" button

**When** I click "Select Annual"  
**Then** the system should:
- Display checkout with annual charge: €99.99
- Process payment for annual renewal
- Update next billing date to Sept 15, 2027
- Send confirmation email with new billing cycle
- Display "Billing cycle changed to Annual" message

---

#### AC 5.2.13: Promotional Code Application

**Given** I have a promotional code: WELCOME50  
**When** I click "Apply Promo Code"  
**Then** the system should:
- Display input field for promo code
- Display "Apply" button

**When** I enter "WELCOME50" and click "Apply"  
**Then** the system should:
- Validate code (if valid: 50% off first month)
- Display success: "Promo code applied! 50% off Premium - €4.99 this month"
- Display discounted price in checkout
- Store code for next billing cycle calculation
- Send confirmation email with discount details

---

#### AC 5.2.14: Payment Method Management

**Given** I am on my Subscription page  
**When** I click "Payment Methods"  
**Then** the system should:
- Display current payment method: "Visa ending in 4242"
- Show expiration: "12/2028"
- Display "Update Payment Method" button
- Display "Add New Payment Method" button
- Display option to remove current method

**When** I click "Update Payment Method"  
**Then** the system should:
- Display secure payment form
- Allow entry of new card details
- Process new card as default for renewals
- Send update confirmation email

---

#### AC 5.2.15: Subscription Management on Mobile

**Given** I am using OmniPitch mobile app  
**When** I navigate to Account > Subscription  
**Then** the system should:
- Display all subscription management options
- Optimize for mobile screen size
- Display plan comparison in scrollable format
- Make upgrade/downgrade/cancel buttons easily tappable (48px minimum)
- Display all information clearly readable on small screens

---

#### AC 5.2.16: Renewal Reminder Notifications

**Given** my subscription renews on October 15, 2026  
**When** October 8, 2026 arrives (7 days before renewal)  
**Then** the system should:
- Send email reminder: "Your Premium subscription renews in 7 days"
- Display in-app notification banner
- Show renewal amount: €9.99
- Include link to manage subscription
- Allow easy cancellation from reminder

---

## 6. Definition of Done (DoD)

For each acceptance criterion to be considered complete, the following must be satisfied:

### Code Quality
- [ ] Code is peer-reviewed and approved
- [ ] Code follows project coding standards and style guide
- [ ] Code includes comments for complex logic
- [ ] No hardcoded values (use configuration)
- [ ] Error handling is implemented

### Testing
- [ ] Unit tests written (minimum 80% code coverage)
- [ ] Integration tests verify end-to-end functionality
- [ ] Acceptance tests pass for all Given-When-Then scenarios
- [ ] Manual testing completed on target devices/browsers
- [ ] Regression tests run and pass

### Documentation
- [ ] Code is documented with docstrings/comments
- [ ] User-facing features are documented
- [ ] API documentation is updated
- [ ] Deployment steps are documented

### Security & Performance
- [ ] Security review completed (OWASP Top 10 checks)
- [ ] Performance benchmarks meet targets
- [ ] SQL injection prevention verified
- [ ] XSS protection verified
- [ ] HTTPS/TLS encryption in place

### Accessibility & Cross-Browser
- [ ] WCAG 2.1 AA compliance verified
- [ ] Tested on Chrome, Firefox, Safari, Edge (latest versions)
- [ ] Tested on iOS and Android (latest versions)
- [ ] Mobile responsiveness verified

### Deployment
- [ ] Code merged to development branch
- [ ] Feature branch deleted after merge
- [ ] Build pipeline passes
- [ ] Staging environment deployment successful
- [ ] Database migrations executed (if applicable)
- [ ] Cache invalidation handled

---

## 7. Test Scenarios & Expected Outcomes

### 7.1 User Registration - Complete Happy Path

**Scenario:** New user registers, verifies email, and logs in

1. User navigates to registration page
2. Fills in all required fields with valid data
3. Accepts terms and conditions
4. Clicks "Create Account"
5. Receives verification email within 1 minute
6. Clicks verification link
7. Email is verified and account activated
8. User can log in with credentials
9. User is redirected to onboarding or dashboard

**Expected Result:** Account created, verified, and accessible ✓

---

### 7.2 Streaming - Quality Switching Stress Test

**Scenario:** User rapidly switches between quality levels during live match

1. Match streaming in 720p
2. User switches to 1080p
3. Before transition completes, user switches to 360p
4. Before that completes, user switches back to 1080p
5. System handles rapid switches without crashing
6. No data corruption or playback errors
7. Video continues streaming without major interruptions
8. Final quality (1080p) is correctly applied

**Expected Result:** System handles rapid quality switching gracefully ✓

---

### 7.3 Multi-Language - Commentary Sync Verification

**Scenario:** User switches languages multiple times during goal sequence

1. Match at 43:15 (42 minutes 15 seconds)
2. User watching English commentary
3. Goal is scored at 43:45
4. English commentator announces goal
5. 1 second into goal announcement, user switches to Spanish
6. Spanish commentator's reaction plays (synced to 43:45)
7. No gap in audio, no replay of already-heard content
8. User switches to German at 43:50
9. German audio continues from correct position
10. All three languages available for replay at 43:45 moment

**Expected Result:** Audio perfectly synced, no content gaps, all languages available ✓

---

### 7.4 Subscription - Upgrade and Feature Unlock

**Scenario:** User upgrades from Free to Premium during live match

1. User on Free tier watching match (limited to 720p)
2. Initiates upgrade to Premium (€9.99/month)
3. Completes payment successfully
4. Receives confirmation email
5. Subscription status updates to "Premium" in account
6. Next refresh of page or player: 1080p option becomes available
7. User selects 1080p and watches in HD
8. Next billing scheduled for 30 days later
9. Billing history shows upgrade transaction

**Expected Result:** Upgrade completes, features unlock, billing configured ✓

---

## 8. Approval & Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | [Name] | __________ | __________ |
| QA Lead | [Name] | __________ | __________ |
| Development Lead | [Name] | __________ | __________ |

---

## 9. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-09-01 | [Author] | Initial acceptance criteria for core features |

---

**End of Acceptance Criteria Document**
