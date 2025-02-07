# Zibal Payment SDK for NodeJS

![Zibal Logo](https://zibal.ir/assets/img/EngLogo.png)

[![npm version](https://img.shields.io/npm/v/zibal.svg)](https://www.npmjs.com/package/zibal)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A robust NodeJS SDK for integrating Zibal payment gateway into your applications.

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [API Reference](#api-reference)
  - [Methods](#methods)
  - [Objects](#objects)
- [Examples](#examples)
- [Error Handling](#error-handling)

## Installation

Install the package using npm:

```bash
npm install --save zibal
```

## Quick Start

```javascript
const Zibal = require('zibal');

// Initialize the SDK
Zibal.init({
    merchant: 'YOUR-MERCHANT-ID',
    callbackUrl: 'https://your-callback-url.com/payment/callback',
    logLevel: 2
});

// Create a payment request
async function createPayment() {
    try {
        const paymentRequest = await Zibal.request(1500, {
            description: 'Payment for Order #123',
            mobile: '09123456789'
        });
        
        // Get payment URL
        const paymentUrl = Zibal.startURL(paymentRequest.trackId);
        console.log('Payment URL:', paymentUrl);
        
        return paymentRequest;
    } catch (error) {
        console.error('Payment request failed:', error);
        throw error;
    }
}

// Verify a payment
async function verifyPayment(trackId) {
    try {
        const verification = await Zibal.verify(trackId);
        console.log('Payment verified:', verification);
        return verification;
    } catch (error) {
        console.error('Payment verification failed:', error);
        throw error;
    }
}
```

## Configuration

### Initialize the SDK

```javascript
Zibal.init({
    merchant: 'YOUR-MERCHANT-ID',      // Required: Your merchant identifier
    callbackUrl: 'YOUR-CALLBACK-URL',  // Required: Payment callback URL
    logLevel: 2                        // Optional: Logging level (default: 2)
});
```

### Log Levels

| Level | Description |
|-------|-------------|
| 0 | No logging (default in production) |
| 1 | Errors only |
| 2 | Errors and info (default) |

## API Reference

### Methods

#### `init(config)`
Initializes the SDK with configuration settings.

```javascript
Zibal.init({
    merchant: 'YOUR-MERCHANT-ID',
    callbackUrl: 'https://your-callback-url.com',
    logLevel: 2
});
```

#### `update(config)`
Updates existing configuration settings.

```javascript
Zibal.update({
    logLevel: 1,
    callbackUrl: 'https://new-callback-url.com'
});
```

#### `request(amount, extras)`
Creates a new payment request.

```javascript
const paymentRequest = await Zibal.request(1500, {
    mobile: '09123456789',
    description: 'Payment for Order #123',
    multiplexingInfos: {...},
    feeMode: 0,
    percentMode: 0
});
```

#### `startURL(trackId)`
Generates the payment gateway URL.

```javascript
const paymentUrl = Zibal.startURL(1533727744287);
```

#### `verify(trackId)`
Verifies a payment transaction.

```javascript
const verification = await Zibal.verify(1533727744287);
```

### Objects

#### Configuration Object
```typescript
interface Config {
    merchant: string;      // Your merchant identifier
    callbackUrl: string;   // Payment callback URL
    logLevel?: number;     // Logging level (0-2)
}
```

#### Extras Object
```typescript
interface Extras {
    mobile?: string;           // Customer mobile number
    description?: string;      // Payment description
    multiplexingInfos?: any;   // Multi-merchant settlement info
    feeMode?: number;          // Fee calculation mode
    percentMode?: number;      // Percentage calculation mode
}
```

#### Request Response
```typescript
interface RequestResponse {
    trackId: number;           // Payment tracking ID
    result: number;           // Result status code
    message: string;          // Status message
    statusMessage: string;    // User-friendly status message
}
```

#### Verify Response
```typescript
interface VerifyResponse {
    paidAt: string;          // Payment timestamp
    amount: number;          // Payment amount
    result: number;          // Result status code
    status: number;          // Payment status
    message: string;         // Status message
    statusMessage: string;   // User-friendly status message
}
```

## Error Handling

The SDK uses Promise-based error handling. Always wrap API calls in try-catch blocks:

```javascript
try {
    const paymentRequest = await Zibal.request(1500);
    // Handle successful request
} catch (error) {
    // Handle errors
    console.error('Payment request failed:', error);
    // error = { 
    //     result: 103, 
    //     message: 'authentication error', 
    //     statusMessage: 'Merchant inactive'
    // }
}
```

## Common Status Codes

| Code | Description |
|------|-------------|
| 100 | Success |
| 102 | Merchant not found |
| 103 | Authentication error |
| 201 | Payment already verified |
| 202 | Payment failed |
| 203 | Payment not verified |

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
