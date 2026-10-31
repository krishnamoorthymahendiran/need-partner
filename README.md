# NEEDS Partner App (Flutter source)

This is a unified partner-side Flutter app with four workspaces:
- Driver Partner
- Food Partner
- Mover Partner
- Service Provider

## Included
- Yellow / black / white NEEDS-inspired UI
- Partner workspace selection
- Online/offline availability toggle
- Incoming request list and sample requests
- Accept / reject actions
- Accepted -> Start job -> Mark completed workflow
- Active requests, history and demo profile screens

## Run on Windows
1. Install Flutter SDK and Android Studio (or Chrome).
2. Extract this project.
3. Open a terminal in this folder.
4. Run:

```bash
flutter clean
flutter pub get
flutter analyze
flutter run -d chrome
```

For Android:
```bash
flutter run
```

## Important backend limitation
This package is a runnable **local demo MVP**. Requests are sample data stored in memory; accepting or completing a job changes only the current app session. It is NOT connected to the customer app, Firestore, live GPS, push notifications, payments, document verification, or an admin panel.

To connect real customer requests securely, first provision the Firestore database for the Firebase project and add trusted server-side booking functions / APIs. Do not allow partner clients to directly change payment amounts or assign themselves arbitrary bookings. After backend endpoints and Firebase partner authentication are available, replace the local `_sampleJobs` source and `_setStatus` handler in `lib/main.dart` with authenticated repository calls and real-time streams.

## Suggested Firestore collections (backend-owned)
- `partnerProfiles/{partnerUid}`: role, approvalStatus, serviceArea, vehicleOrBusinessDetails
- `partnerApplications/{applicationId}`: submitted KYC/business details, review status
- `serviceRequests/{requestId}`: customerUid, serviceType, status, assignedPartnerUid, pickup, destination, amount, timestamps
- `serviceRequests/{requestId}/events/{eventId}`: audit trail of accepted/rejected/started/completed transitions

Only trusted backend code should assign partners, validate transitions, and create financial records.
