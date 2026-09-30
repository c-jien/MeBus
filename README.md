# MeBus

An iOS bus-arrival app for Singapore, built with SwiftUI and a small Flask backend using LTA DataMall.

## Features

- Nearby bus stops using device location
- Arrival information for a selected bus stop
- Saved favorite stops
- Search across Singapore bus stops

## Requirements

- Xcode with an iOS 17.4 or newer SDK
- Python 3.10 or newer for the backend
- Your own [LTA DataMall](https://datamall.lta.gov.sg/) account key
- An HTTPS endpoint reachable by the iOS app

## Run the backend

```sh
git clone https://github.com/c-jien/MeBus.git
cd MeBus/API
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
cp .env.example .env
```

Set `LTA_ACCOUNT_KEY` in `.env` to your own key. Load it into the environment and start the local development server:

```sh
set -a
. ./.env
set +a
python bus_api.py
```

The server listens on port **6001** and serves `GET /bus-arrival/<bus-stop-code>`. It refuses to start without a key. `.env` is ignored by Git; `.env.example` contains only a blank template.

For a temporary development HTTPS endpoint, run this in another terminal:

```sh
ngrok http 6001
```

Flask's built-in server is for local development. A hosted deployment needs an appropriate WSGI server and HTTPS configuration.

## Run the iOS app

1. Open `MeBus.xcodeproj` in Xcode.
2. In **Signing & Capabilities**, select your own development team if running on a device.
3. In `MeBus-v2/Components/BusStopDetail.swift`, replace the `urlString` placeholder inside `fetchBusArrivalData()` with your complete endpoint, including the selected stop code, for example:

   ```swift
   let urlString = "https://your-api.example.com/bus-arrival/\(busStopCode)"
   ```

4. Choose an iOS 17.4+ simulator or device and build the app. Allow location access to use nearby stops.

The LTA key stays on the backend; do not embed it in the Swift app. Revoke any previously exposed key before deploying a replacement. Removing a key from the latest source does not revoke it or remove historical copies.

This is a personal learning project originally developed in 2024. The setup describes the checked-in project; current LTA service compatibility and device behavior should be verified before deployment.

## Images

   <img src="https://github.com/meokdev/MeBus/assets/62682756/6e67bdcb-d2d1-4750-afe8-47f4abbc32f1" width="200">
   <img src="https://github.com/meokdev/MeBus/assets/62682756/460b8ee5-ec88-418f-a48d-e9fc06b86701" width="200">
   <img src="https://github.com/meokdev/MeBus/assets/62682756/2b3ea914-2ab7-4eb9-ae5c-4bd807c94c20" width="200">
   <img src="https://github.com/meokdev/MeBus/assets/62682756/a8f1ec3f-4584-41b3-88ce-7af26c5eb6a7" width="200">
   <img src="https://github.com/meokdev/MeBus/assets/62682756/f54ea11e-ea21-4da6-986f-e6b3800abbc9" width="200">


## License

MIT — see [LICENSE](LICENSE).
