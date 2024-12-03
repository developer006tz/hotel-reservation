# Complete API Documentation with Examples
Base URL: `https://api.yourdomain.com/v1`

## Authentication APIs

### 1. User Registration
```http
POST /auth/register
Content-Type: application/json
```

Request:
```json
{
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "phone": "255123456789",
    "password": "SecureP@ssw0rd",
    "confirmPassword": "SecureP@ssw0rd",
    "idType": "NIDA",
    "idNumber": "19990101-12345-00000-00"
}
```

Success Response (201 Created):
```json
{
    "status": "success",
    "data": {
        "userId": "550e8400-e29b-41d4-a716-446655440000",
        "email": "john@example.com",
        "firstName": "John",
        "lastName": "Doe",
        "emailVerified": false,
        "phoneVerified": false,
        "message": "Registration successful. Please verify your email."
    }
}
```

### 2. User Login
```http
POST /auth/login
Content-Type: application/json
```

Request:
```json
{
    "email": "john@example.com",
    "password": "SecureP@ssw0rd"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "accessToken": "eyJhbGciOiJIUzI1NiIs...",
        "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
        "user": {
            "id": "550e8400-e29b-41d4-a716-446655440000",
            "email": "john@example.com",
            "firstName": "John",
            "lastName": "Doe",
            "role": "GUEST"
        }
    }
}
```

## Property Management APIs

### 1. List Properties
```http
GET /properties?page=1&limit=10&location=dar-es-salaam&type=HOTEL
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "properties": [
            {
                "id": "550e8400-e29b-41d4-a716-446655440001",
                "name": "Serena Hotel",
                "slug": "serena-hotel-dar",
                "type": "HOTEL",
                "description": "Luxury hotel in downtown Dar es Salaam",
                "starRating": 5,
                "mainImage": "https://storage.example.com/images/serena-main.jpg",
                "location": {
                    "region": "Dar es Salaam",
                    "district": "Ilala",
                    "coordinates": {
                        "latitude": -6.8162,
                        "longitude": 39.2887
                    }
                },
                "basePrice": 250000,
                "amenities": [
                    {
                        "id": "amenity-1",
                        "name": "Swimming Pool",
                        "icon": "pool"
                    }
                ]
            }
        ]
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 10,
            "total": 100,
            "pages": 10
        }
    }
}
```

### 2. Get Single Property
```http
GET /properties/550e8400-e29b-41d4-a716-446655440001
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "property": {
            "id": "550e8400-e29b-41d4-a716-446655440001",
            "name": "Serena Hotel",
            "slug": "serena-hotel-dar",
            "type": "HOTEL",
            "description": "Luxury hotel in downtown Dar es Salaam",
            "longDescription": "Detailed HTML description...",
            "starRating": 5,
            "license": "TZ-HOTEL-2024-001",
            "tinNumber": "123-456-789",
            "images": [
                {
                    "id": "img-1",
                    "url": "https://storage.example.com/images/serena-1.jpg",
                    "altText": "Hotel Exterior",
                    "type": "EXTERIOR",
                    "displayOrder": 1
                }
            ],
            "location": {
                "region": "Dar es Salaam",
                "district": "Ilala",
                "ward": "Upanga",
                "street": "Ohio Street",
                "coordinates": {
                    "latitude": -6.8162,
                    "longitude": 39.2887
                }
            },
            "amenities": [
                {
                    "id": "amenity-1",
                    "name": "Swimming Pool",
                    "category": "PROPERTY",
                    "icon": "pool"
                }
            ],
            "rooms": [
                {
                    "id": "room-1",
                    "name": "Deluxe Ocean View",
                    "basePrice": 250000,
                    "capacity": {
                        "adults": 2,
                        "children": 1
                    }
                }
            ],
            "policies": {
                "checkIn": "14:00",
                "checkOut": "11:00",
                "cancellation": "Free cancellation up to 24 hours before check-in"
            },
            "contact": {
                "phone": "+255123456789",
                "email": "reservations@serena.co.tz",
                "whatsapp": "+255123456789"
            }
        }
    }
}
```

### 3. Create Property (Admin Only)
```http
POST /properties
Authorization: Bearer {admin_token}
Content-Type: application/json
```

Request:
```json
{
    "name": "New Beach Resort",
    "type": "RESORT",
    "description": "Beautiful beach resort in Zanzibar",
    "longDescription": "Detailed HTML description...",
    "license": "TZ-RESORT-2024-002",
    "tinNumber": "987-654-321",
    "location": {
        "region": "Zanzibar",
        "district": "North",
        "ward": "Nungwi",
        "street": "Beach Road",
        "coordinates": {
            "latitude": -5.7262,
            "longitude": 39.2962
        }
    },
    "contact": {
        "phone": "+255987654321",
        "email": "info@newbeach.co.tz",
        "whatsapp": "+255987654321"
    },
    "policies": {
        "checkIn": "15:00",
        "checkOut": "10:00",
        "cancellation": "48-hour cancellation policy"
    }
}
```

Success Response (201 Created):
```json
{
    "status": "success",
    "data": {
        "property": {
            "id": "550e8400-e29b-41d4-a716-446655440002",
            "name": "New Beach Resort",
            "slug": "new-beach-resort-zanzibar",
            "createdAt": "2024-12-03T10:00:00Z"
        }
    }
}
```

## Room Management APIs

### 1. List Rooms for Property
```http
GET /properties/550e8400-e29b-41d4-a716-446655440001/rooms
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "rooms": [
            {
                "id": "room-uuid-1",
                "name": "Deluxe Ocean View",
                "slug": "deluxe-ocean-view",
                "roomNumber": "401",
                "floorNumber": 4,
                "basePrice": 250000,
                "description": "Spacious room with ocean view",
                "roomSize": 45,
                "sizeUnit": "SQM",
                "bedType": "KING",
                "numBeds": 1,
                "maxOccupancy": {
                    "adults": 2,
                    "children": 1
                },
                "amenities": [
                    {
                        "id": "amenity-1",
                        "name": "Air Conditioning",
                        "icon": "ac"
                    }
                ],
                "images": [
                    {
                        "id": "img-1",
                        "url": "https://storage.example.com/rooms/deluxe-1.jpg",
                        "altText": "Room Interior",
                        "type": "MAIN"
                    }
                ],
                "status": "AVAILABLE"
            }
        ]
    }
}
```

# Booking APIs Documentation

## Create Booking

### Initial Booking Creation
```http
POST /bookings
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "propertyId": "550e8400-e29b-41d4-a716-446655440001",
    "roomId": "room-uuid-1",
    "checkIn": "2024-12-24",
    "checkOut": "2024-12-27",
    "guests": {
        "adults": 2,
        "children": 1
    },
    "specialRequests": "Ground floor room preferred, early check-in if possible",
    "guestDetails": {
        "firstName": "John",
        "lastName": "Doe",
        "email": "john@example.com",
        "phone": "255123456789",
        "idType": "PASSPORT",
        "idNumber": "AB123456"
    }
}
```

Success Response (201 Created):
```json
{
    "status": "success",
    "data": {
        "booking": {
            "id": "booking-uuid-1",
            "bookingNumber": "BK24120001",
            "propertyDetails": {
                "id": "550e8400-e29b-41d4-a716-446655440001",
                "name": "Serena Hotel",
                "location": "Dar es Salaam"
            },
            "roomDetails": {
                "id": "room-uuid-1",
                "name": "Deluxe Ocean View",
                "roomNumber": "401"
            },
            "dates": {
                "checkIn": "2024-12-24",
                "checkOut": "2024-12-27",
                "nights": 3
            },
            "guests": {
                "adults": 2,
                "children": 1
            },
            "pricing": {
                "basePrice": 250000,
                "taxAmount": 40000,
                "totalAmount": 290000,
                "currency": "TZS"
            },
            "status": "PENDING",
            "paymentStatus": "AWAITING_PAYMENT",
            "expiresAt": "2024-12-03T11:00:00Z",
            "createdAt": "2024-12-03T10:00:00Z"
        },
        "paymentInstructions": {
            "amount": 290000,
            "currency": "TZS",
            "paymentMethods": [
                {
                    "method": "MPESA",
                    "instructions": "Dial *150*00#..."
                },
                {
                    "method": "TIGOPESA",
                    "instructions": "Dial *150*01#..."
                }
            ]
        }
    }
}
```

## Retrieve Booking

### Get Booking Details
```http
GET /bookings/booking-uuid-1
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "booking": {
            "id": "booking-uuid-1",
            "bookingNumber": "BK24120001",
            "status": "CONFIRMED",
            "propertyDetails": {
                "id": "550e8400-e29b-41d4-a716-446655440001",
                "name": "Serena Hotel",
                "address": "Ohio Street, Dar es Salaam",
                "phone": "+255123456789",
                "email": "reservations@serena.co.tz"
            },
            "roomDetails": {
                "id": "room-uuid-1",
                "name": "Deluxe Ocean View",
                "roomNumber": "401",
                "amenities": [
                    {
                        "name": "Air Conditioning",
                        "icon": "ac"
                    }
                ]
            },
            "dates": {
                "checkIn": "2024-12-24",
                "checkOut": "2024-12-27",
                "nights": 3,
                "policies": {
                    "checkInTime": "14:00",
                    "checkOutTime": "11:00"
                }
            },
            "guests": {
                "adults": 2,
                "children": 1,
                "primaryGuest": {
                    "firstName": "John",
                    "lastName": "Doe",
                    "email": "john@example.com",
                    "phone": "255123456789"
                }
            },
            "pricing": {
                "breakdown": {
                    "basePrice": 250000,
                    "taxAmount": 40000,
                    "totalAmount": 290000
                },
                "currency": "TZS",
                "paymentStatus": "PAID",
                "paymentMethod": "MPESA"
            },
            "specialRequests": "Ground floor room preferred, early check-in if possible",
            "timeline": [
                {
                    "status": "PENDING",
                    "timestamp": "2024-12-03T10:00:00Z"
                },
                {
                    "status": "PAYMENT_RECEIVED",
                    "timestamp": "2024-12-03T10:15:00Z"
                },
                {
                    "status": "CONFIRMED",
                    "timestamp": "2024-12-03T10:16:00Z"
                }
            ]
        }
    }
}
```

## List User Bookings

### Get All User Bookings
```http
GET /bookings?status=all&page=1&limit=10
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "bookings": [
            {
                "id": "booking-uuid-1",
                "bookingNumber": "BK24120001",
                "propertyName": "Serena Hotel",
                "roomType": "Deluxe Ocean View",
                "checkIn": "2024-12-24",
                "checkOut": "2024-12-27",
                "status": "CONFIRMED",
                "totalAmount": 290000,
                "paymentStatus": "PAID"
            }
        ]
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 10,
            "total": 1,
            "pages": 1
        }
    }
}
```

## Cancel Booking

### Cancel Existing Booking
```http
POST /bookings/booking-uuid-1/cancel
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "reason": "Change of plans",
    "cancellationNotes": "Unable to travel on selected dates"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "booking": {
            "id": "booking-uuid-1",
            "bookingNumber": "BK24120001",
            "status": "CANCELLED",
            "cancellation": {
                "reason": "Change of plans",
                "timestamp": "2024-12-03T15:00:00Z",
                "refundDetails": {
                    "amount": 261000,
                    "currency": "TZS",
                    "refundMethod": "MPESA",
                    "estimatedProcessingDays": 3,
                    "refundStatus": "PROCESSING"
                }
            }
        }
    }
}
```

## Modify Booking

### Update Booking Details
```http
PUT /bookings/booking-uuid-1
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "guests": {
        "adults": 2,
        "children": 2
    },
    "specialRequests": "Updated: Need extra bed for child"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "booking": {
            "id": "booking-uuid-1",
            "bookingNumber": "BK24120001",
            "updates": {
                "previousGuests": {
                    "adults": 2,
                    "children": 1
                },
                "newGuests": {
                    "adults": 2,
                    "children": 2
                },
                "pricingAdjustment": {
                    "additionalCharges": 50000,
                    "reason": "Extra bed for child"
                }
            },
            "newTotalAmount": 340000,
            "paymentRequired": 50000
        }
    }
}
```

## Check-in Processing

### Process Guest Check-in
```http
POST /bookings/booking-uuid-1/check-in
Authorization: Bearer {staff_token}
Content-Type: application/json
```

Request:
```json
{
    "identificationDocument": {
        "type": "PASSPORT",
        "number": "AB123456",
        "expiryDate": "2025-12-31"
    },
    "signature": "base64_encoded_signature",
    "roomNumber": "401",
    "keyCardNumber": "KC123456"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "checkIn": {
            "bookingId": "booking-uuid-1",
            "status": "CHECKED_IN",
            "timestamp": "2024-12-24T14:30:00Z",
            "roomDetails": {
                "roomNumber": "401",
                "keyCardNumber": "KC123456",
                "wifiDetails": {
                    "networkName": "SerenaGuest",
                    "password": "Welcome2024"
                }
            },
            "checkedInBy": "staff-name"
        }
    }
}
```

# Payment APIs Documentation - Tanzania Hotel Booking System

## Payment Initialization

### Initialize Payment
```http
POST /payments/initialize
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "bookingId": "booking-uuid-1",
    "amount": 290000,
    "currency": "TZS",
    "paymentMethod": "SELCOM",
    "paymentOption": "MPESA",
    "customerPhone": "255123456789",
    "customerEmail": "john@example.com",
    "customerName": "John Doe"
}
```

Success Response (201 Created):
```json
{
    "status": "success",
    "data": {
        "paymentId": "payment-uuid-1",
        "paymentNumber": "PAY24120001",
        "transactionDetails": {
            "orderId": "SO12345678",
            "transid": "TXN123456",
            "reference": "REF123456",
            "paymentToken": "TOKEN123456",
            "paymentUrl": "https://checkout.selcom.co.tz/pay/TOKEN123456",
            "expiresAt": "2024-12-03T11:00:00Z"
        },
        "amount": 290000,
        "currency": "TZS",
        "paymentInstructions": {
            "MPESA": {
                "dialCode": "*150*00#",
                "merchantCode": "123456",
                "accountNumber": "0123456789"
            }
        },
        "status": "PENDING"
    }
}
```

## Payment Verification

### Verify Payment Status
```http
GET /payments/payment-uuid-1/status
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "payment": {
            "id": "payment-uuid-1",
            "paymentNumber": "PAY24120001",
            "status": "SUCCESSFUL",
            "amount": 290000,
            "currency": "TZS",
            "paymentMethod": "MPESA",
            "transactionDetails": {
                "providerReference": "MPESA123456",
                "customerPhone": "255123456789",
                "timestamp": "2024-12-03T10:15:00Z"
            },
            "booking": {
                "id": "booking-uuid-1",
                "bookingNumber": "BK24120001",
                "status": "CONFIRMED"
            }
        }
    }
}
```

## Selcom Webhook Handler

### Handle Payment Notification
```http
POST /payments/webhook/selcom
X-Signature: {webhook_signature}
Content-Type: application/json
```

Request:
```json
{
    "transid": "TXN123456",
    "reference": "REF123456",
    "status": "SUCCESSFUL",
    "amount": "290000",
    "msisdn": "255123456789",
    "operator": "Vodacom",
    "message": "Payment successful",
    "utilityref": "SO12345678",
    "timestamp": "2024-12-03T10:15:00Z"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "message": "Webhook processed successfully"
}
```

## Payment Receipts

### Get Payment Receipt
```http
GET /payments/payment-uuid-1/receipt
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "receipt": {
            "receiptNumber": "RCP24120001",
            "paymentNumber": "PAY24120001",
            "bookingNumber": "BK24120001",
            "dateIssued": "2024-12-03T10:16:00Z",
            "propertyDetails": {
                "name": "Serena Hotel",
                "tinNumber": "123-456-789",
                "address": "Ohio Street, Dar es Salaam"
            },
            "customerDetails": {
                "name": "John Doe",
                "email": "john@example.com",
                "phone": "255123456789"
            },
            "paymentDetails": {
                "amount": 290000,
                "currency": "TZS",
                "breakdown": {
                    "roomCharge": 250000,
                    "tax": 40000
                },
                "paymentMethod": "MPESA",
                "transactionReference": "MPESA123456"
            },
            "downloadUrl": "https://api.example.com/receipts/RCP24120001.pdf"
        }
    }
}
```

## Refund Processing

### Process Refund
```http
POST /payments/payment-uuid-1/refund
Authorization: Bearer {admin_token}
Content-Type: application/json
```

Request:
```json
{
    "amount": 261000,
    "reason": "Booking cancellation",
    "refundMethod": {
        "type": "MPESA",
        "phone": "255123456789"
    },
    "notes": "Cancellation within policy period - 90% refund"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "refund": {
            "id": "refund-uuid-1",
            "refundNumber": "REF24120001",
            "originalPayment": {
                "paymentNumber": "PAY24120001",
                "amount": 290000
            },
            "refundAmount": 261000,
            "status": "PROCESSING",
            "estimatedProcessingDays": 3,
            "refundDetails": {
                "method": "MPESA",
                "phone": "255123456789",
                "reference": "RFND123456"
            },
            "timeline": [
                {
                    "status": "INITIATED",
                    "timestamp": "2024-12-03T15:00:00Z"
                },
                {
                    "status": "PROCESSING",
                    "timestamp": "2024-12-03T15:01:00Z"
                }
            ]
        }
    }
}
```

## Payment History

### Get Payment History
```http
GET /payments?startDate=2024-12-01&endDate=2024-12-31
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "payments": [
            {
                "id": "payment-uuid-1",
                "paymentNumber": "PAY24120001",
                "bookingNumber": "BK24120001",
                "amount": 290000,
                "currency": "TZS",
                "paymentMethod": "MPESA",
                "status": "SUCCESSFUL",
                "timestamp": "2024-12-03T10:15:00Z"
            }
        ]
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 10,
            "total": 1,
            "pages": 1
        },
        "summary": {
            "totalAmount": 290000,
            "successfulPayments": 1,
            "failedPayments": 0
        }
    }
}
```



# Review APIs Documentation - Tanzania Hotel Booking System

## Submit Review

### Create New Review
```http
POST /properties/{propertyId}/reviews
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "bookingId": "booking-uuid-1",
    "rating": 5,
    "title": "Exceptional Stay in Dar es Salaam",
    "comment": "Our stay at Serena Hotel was absolutely wonderful. The staff was very professional and attentive.",
    "categories": {
        "cleanliness": 5,
        "service": 5,
        "location": 5,
        "amenities": 4,
        "valueForMoney": 4
    },
    "images": [
        {
            "image": "base64_encoded_image_1",
            "caption": "Beautiful ocean view from room"
        },
        {
            "image": "base64_encoded_image_2",
            "caption": "Restaurant area"
        }
    ],
    "recommendationScore": 9,
    "stayDetails": {
        "travelType": "BUSINESS",
        "groupType": "SOLO",
        "stayDate": "2024-12"
    }
}
```

Success Response (201 Created):
```json
{
    "status": "success",
    "data": {
        "review": {
            "id": "review-uuid-1",
            "propertyId": "property-uuid-1",
            "bookingId": "booking-uuid-1",
            "verifiedStay": true,
            "rating": 5,
            "title": "Exceptional Stay in Dar es Salaam",
            "comment": "Our stay at Serena Hotel was absolutely wonderful...",
            "categories": {
                "cleanliness": 5,
                "service": 5,
                "location": 5,
                "amenities": 4,
                "valueForMoney": 4
            },
            "images": [
                {
                    "id": "image-uuid-1",
                    "url": "https://storage.example.com/reviews/image-1.jpg",
                    "caption": "Beautiful ocean view from room"
                }
            ],
            "status": "PENDING_APPROVAL",
            "createdAt": "2024-12-03T10:00:00Z"
        }
    }
}
```

## Retrieve Reviews

### Get Property Reviews
```http
GET /properties/{propertyId}/reviews?page=1&limit=10&sort=recent
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "summary": {
            "averageRating": 4.5,
            "totalReviews": 150,
            "categoryAverages": {
                "cleanliness": 4.6,
                "service": 4.7,
                "location": 4.8,
                "amenities": 4.3,
                "valueForMoney": 4.2
            },
            "recommendationScore": 8.5
        },
        "reviews": [
            {
                "id": "review-uuid-1",
                "verifiedStay": true,
                "rating": 5,
                "title": "Exceptional Stay in Dar es Salaam",
                "comment": "Our stay at Serena Hotel was absolutely wonderful...",
                "categories": {
                    "cleanliness": 5,
                    "service": 5,
                    "location": 5,
                    "amenities": 4,
                    "valueForMoney": 4
                },
                "images": [
                    {
                        "url": "https://storage.example.com/reviews/image-1.jpg",
                        "caption": "Beautiful ocean view from room"
                    }
                ],
                "guest": {
                    "name": "John D.",
                    "stayDate": "December 2024",
                    "travelType": "BUSINESS",
                    "groupType": "SOLO"
                },
                "helpful": {
                    "count": 12,
                    "hasVoted": false
                },
                "propertyResponse": {
                    "response": "Thank you for your wonderful review...",
                    "respondedAt": "2024-12-04T09:00:00Z",
                    "respondentName": "Hotel Manager"
                },
                "createdAt": "2024-12-03T10:00:00Z"
            }
        ]
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 10,
            "total": 150,
            "pages": 15
        }
    }
}
```

## Property Response

### Respond to Review
```http
POST /reviews/{reviewId}/respond
Authorization: Bearer {property_token}
Content-Type: application/json
```

Request:
```json
{
    "response": "Dear valued guest, thank you for your wonderful review. We are delighted to hear that you enjoyed your stay with us...",
    "respondentName": "James Wilson",
    "respondentTitle": "Hotel Manager"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "response": {
            "reviewId": "review-uuid-1",
            "response": "Dear valued guest, thank you for your wonderful review...",
            "respondentName": "James Wilson",
            "respondentTitle": "Hotel Manager",
            "respondedAt": "2024-12-04T09:00:00Z"
        }
    }
}
```

## Review Management

### Update Review
```http
PUT /reviews/{reviewId}
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "rating": 4,
    "title": "Updated: Great Stay in Dar es Salaam",
    "comment": "Updated review comment...",
    "categories": {
        "cleanliness": 4,
        "service": 4,
        "location": 5,
        "amenities": 4,
        "valueForMoney": 4
    }
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "review": {
            "id": "review-uuid-1",
            "rating": 4,
            "title": "Updated: Great Stay in Dar es Salaam",
            "comment": "Updated review comment...",
            "categories": {
                "cleanliness": 4,
                "service": 4,
                "location": 5,
                "amenities": 4,
                "valueForMoney": 4
            },
            "updatedAt": "2024-12-04T10:00:00Z"
        }
    }
}
```

## Review Moderation

### Moderate Review (Admin Only)
```http
POST /reviews/{reviewId}/moderate
Authorization: Bearer {admin_token}
Content-Type: application/json
```

Request:
```json
{
    "action": "APPROVE",
    "moderationNotes": "Review meets content guidelines",
    "moderatedContent": {
        "title": null,
        "comment": null
    }
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "moderation": {
            "reviewId": "review-uuid-1",
            "status": "APPROVED",
            "moderatedBy": "admin-uuid-1",
            "moderationNotes": "Review meets content guidelines",
            "moderatedAt": "2024-12-04T11:00:00Z"
        }
    }
}
```

## Review Interactions

### Mark Review as Helpful
```http
POST /reviews/{reviewId}/helpful
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "helpful": {
            "reviewId": "review-uuid-1",
            "helpfulCount": 13,
            "hasVoted": true
        }
    }
}
```

## Review Analytics

### Get Review Analytics
```http
GET /properties/{propertyId}/reviews/analytics
Authorization: Bearer {property_token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "analytics": {
            "overall": {
                "averageRating": 4.5,
                "totalReviews": 150,
                "responseRate": 95,
                "averageResponseTime": "24 hours"
            },
            "trends": {
                "monthly": [
                    {
                        "month": "December 2024",
                        "averageRating": 4.6,
                        "reviewCount": 25
                    }
                ],
                "categories": {
                    "cleanliness": {
                        "trend": "IMPROVING",
                        "currentScore": 4.6,
                        "previousScore": 4.4
                    }
                }
            },
            "sentimentAnalysis": {
                "positive": 75,
                "neutral": 20,
                "negative": 5,
                "topKeywords": [
                    {
                        "word": "service",
                        "count": 89,
                        "sentiment": "positive"
                    }
                ]
            }
        }
    }
}
```



# User Profile APIs Documentation - Tanzania Hotel Booking System

## Profile Management

### Get User Profile
```http
GET /users/profile
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "profile": {
            "id": "user-uuid-1",
            "personalInfo": {
                "firstName": "John",
                "lastName": "Doe",
                "email": "john@example.com",
                "phone": "255123456789",
                "dateOfBirth": "1990-01-01",
                "nationality": "Tanzanian",
                "preferredLanguage": "en",
                "avatar": "https://storage.example.com/avatars/user-1.jpg"
            },
            "verificationStatus": {
                "email": true,
                "phone": true,
                "identity": true
            },
            "identificationDetails": {
                "type": "NIDA",
                "number": "19900101-12345-00000-00",
                "expiryDate": "2030-01-01",
                "verifiedAt": "2024-01-15T10:00:00Z"
            },
            "address": {
                "street": "123 Nyerere Road",
                "ward": "Upanga West",
                "district": "Ilala",
                "region": "Dar es Salaam",
                "country": "Tanzania"
            },
            "preferences": {
                "currency": "TZS",
                "notifications": {
                    "email": true,
                    "sms": true,
                    "whatsapp": true
                },
                "roomPreferences": {
                    "smoking": false,
                    "floorPreference": "HIGH",
                    "bedType": "KING"
                }
            },
            "statistics": {
                "totalBookings": 5,
                "completedStays": 4,
                "reviewsWritten": 3,
                "totalSpent": 1450000,
                "memberSince": "2024-01-01"
            }
        }
    }
}
```

### Update Profile Information
```http
PUT /users/profile
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "personalInfo": {
        "firstName": "John",
        "lastName": "Doe",
        "phone": "255123456789",
        "dateOfBirth": "1990-01-01",
        "nationality": "Tanzanian"
    },
    "address": {
        "street": "123 Nyerere Road",
        "ward": "Upanga West",
        "district": "Ilala",
        "region": "Dar es Salaam"
    },
    "preferences": {
        "notifications": {
            "sms": true,
            "whatsapp": true
        },
        "roomPreferences": {
            "smoking": false,
            "floorPreference": "HIGH"
        }
    }
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "message": "Profile updated successfully",
        "updatedFields": [
            "personalInfo",
            "address",
            "preferences"
        ],
        "updatedAt": "2024-12-03T10:00:00Z"
    }
}
```

### Upload Profile Picture
```http
POST /users/profile/avatar
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

Request:
```
Form Data:
- avatar: [image file]
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "avatar": {
            "url": "https://storage.example.com/avatars/user-1.jpg",
            "updatedAt": "2024-12-03T10:00:00Z"
        }
    }
}
```

## Identity Verification

### Submit Identity Verification
```http
POST /users/verify-identity
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

Request:
```
Form Data:
- idType: "NIDA"
- idNumber: "19900101-12345-00000-00"
- idFront: [image file]
- idBack: [image file]
- selfie: [image file]
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "verification": {
            "id": "verification-uuid-1",
            "status": "PENDING",
            "submittedAt": "2024-12-03T10:00:00Z",
            "estimatedProcessingTime": "24 hours"
        }
    }
}
```

## Security Settings

### Change Password
```http
POST /users/security/change-password
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "currentPassword": "OldPassword123",
    "newPassword": "NewSecurePassword123",
    "confirmPassword": "NewSecurePassword123"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "message": "Password changed successfully",
        "changedAt": "2024-12-03T10:00:00Z"
    }
}
```

### Two-Factor Authentication Settings
```http
POST /users/security/2fa/setup
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "secret": "JBSWY3DPEHPK3PXP",
        "qrCode": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgA...",
        "recoveryCodes": [
            "1234-5678-9012",
            "2345-6789-0123"
        ]
    }
}
```

## Booking History and Preferences

### Get Booking History
```http
GET /users/bookings?page=1&limit=10&status=all
Authorization: Bearer {token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "bookings": [
            {
                "id": "booking-uuid-1",
                "propertyName": "Serena Hotel",
                "checkIn": "2024-12-24",
                "checkOut": "2024-12-27",
                "status": "CONFIRMED",
                "amount": 290000,
                "roomType": "Deluxe Ocean View",
                "bookingNumber": "BK24120001"
            }
        ],
        "statistics": {
            "totalBookings": 5,
            "totalSpent": 1450000,
            "averageStayDuration": 3,
            "favoriteProperty": "Serena Hotel",
            "mostVisitedCity": "Dar es Salaam"
        }
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 10,
            "total": 5,
            "pages": 1
        }
    }
}
```

### Update Notification Preferences
```http
PUT /users/preferences/notifications
Authorization: Bearer {token}
Content-Type: application/json
```

Request:
```json
{
    "email": {
        "bookingConfirmations": true,
        "specialOffers": false,
        "newsletters": false
    },
    "sms": {
        "bookingReminders": true,
        "securityAlerts": true
    },
    "whatsapp": {
        "bookingUpdates": true,
        "customerSupport": true
    }
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "preferences": {
            "email": {
                "bookingConfirmations": true,
                "specialOffers": false,
                "newsletters": false
            },
            "sms": {
                "bookingReminders": true,
                "securityAlerts": true
            },
            "whatsapp": {
                "bookingUpdates": true,
                "customerSupport": true
            }
        },
        "updatedAt": "2024-12-03T10:00:00Z"
    }
}
```




# Admin APIs Documentation - Tanzania Hotel Booking System

## Dashboard Analytics

### Get Overall System Analytics
```http
GET /admin/analytics/dashboard
Authorization: Bearer {admin_token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "overview": {
            "totalBookings": {
                "count": 1250,
                "growth": 15.5,
                "timeline": [
                    {
                        "date": "2024-12",
                        "count": 150
                    }
                ]
            },
            "revenue": {
                "total": 275000000,
                "currency": "TZS",
                "growth": 22.3,
                "breakdown": {
                    "roomRevenue": 250000000,
                    "additionalCharges": 25000000
                }
            },
            "properties": {
                "total": 45,
                "active": 42,
                "pending": 3,
                "byType": {
                    "HOTEL": 20,
                    "LODGE": 15,
                    "GUESTHOUSE": 8,
                    "MOTEL": 2
                }
            },
            "users": {
                "total": 5000,
                "active": 4800,
                "newThisMonth": 150
            }
        },
        "occupancyRates": {
            "average": 75.5,
            "byPropertyType": {
                "HOTEL": 82.3,
                "LODGE": 78.5,
                "GUESTHOUSE": 65.8,
                "MOTEL": 60.2
            }
        },
        "customerSatisfaction": {
            "overallRating": 4.5,
            "responseRate": 92.5,
            "reviewCount": 850
        }
    }
}
```

## Property Management

### List All Properties (Admin View)
```http
GET /admin/properties?page=1&limit=20&status=all
Authorization: Bearer {admin_token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "properties": [
            {
                "id": "property-uuid-1",
                "name": "Serena Hotel",
                "type": "HOTEL",
                "location": {
                    "region": "Dar es Salaam",
                    "district": "Ilala"
                },
                "status": "ACTIVE",
                "verificationStatus": "VERIFIED",
                "owner": {
                    "id": "owner-uuid-1",
                    "name": "Serena Group",
                    "contact": "255123456789"
                },
                "metrics": {
                    "totalRooms": 150,
                    "activeBookings": 98,
                    "occupancyRate": 82.5,
                    "averageRating": 4.7,
                    "monthlyRevenue": 150000000
                },
                "compliance": {
                    "licenseStatus": "VALID",
                    "licenseExpiry": "2025-12-31",
                    "taxCompliant": true,
                    "lastInspection": "2024-11-15"
                }
            }
        ]
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 20,
            "total": 45,
            "pages": 3
        }
    }
}
```

### Property Verification Management
```http
POST /admin/properties/{propertyId}/verify
Authorization: Bearer {admin_token}
Content-Type: application/json
```

Request:
```json
{
    "verificationStatus": "VERIFIED",
    "inspectionDetails": {
        "inspectorId": "inspector-uuid-1",
        "inspectionDate": "2024-12-03",
        "notes": "All requirements met",
        "complianceChecklist": {
            "license": true,
            "safety": true,
            "hygiene": true,
            "facilities": true
        }
    },
    "documents": {
        "businessLicense": "verified",
        "taxClearance": "verified",
        "safetyCompliance": "verified"
    }
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "verification": {
            "propertyId": "property-uuid-1",
            "status": "VERIFIED",
            "verifiedBy": "admin-uuid-1",
            "verifiedAt": "2024-12-03T10:00:00Z",
            "validUntil": "2025-12-03T10:00:00Z",
            "certificate": {
                "number": "CERT2024001",
                "downloadUrl": "https://api.example.com/certificates/CERT2024001.pdf"
            }
        }
    }
}
```

## User Management

### User Administration
```http
GET /admin/users?page=1&limit=20&role=all
Authorization: Bearer {admin_token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "users": [
            {
                "id": "user-uuid-1",
                "email": "john@example.com",
                "role": "GUEST",
                "status": "ACTIVE",
                "verificationStatus": {
                    "email": true,
                    "phone": true,
                    "identity": true
                },
                "activity": {
                    "lastLogin": "2024-12-02T15:30:00Z",
                    "totalBookings": 5,
                    "totalSpent": 1450000,
                    "registeredOn": "2024-01-01T10:00:00Z"
                },
                "flags": {
                    "hasDisputes": false,
                    "specialRequirements": false
                }
            }
        ]
    },
    "meta": {
        "pagination": {
            "page": 1,
            "limit": 20,
            "total": 5000,
            "pages": 250
        }
    }
}
```

## Booking Management

### Administrative Booking Operations
```http
GET /admin/bookings?page=1&limit=20&status=all
Authorization: Bearer {admin_token}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "bookings": [
            {
                "id": "booking-uuid-1",
                "bookingNumber": "BK24120001",
                "property": {
                    "id": "property-uuid-1",
                    "name": "Serena Hotel"
                },
                "guest": {
                    "id": "user-uuid-1",
                    "name": "John Doe",
                    "contact": "255123456789"
                },
                "dates": {
                    "checkIn": "2024-12-24",
                    "checkOut": "2024-12-27"
                },
                "status": "CONFIRMED",
                "payment": {
                    "amount": 290000,
                    "status": "PAID",
                    "method": "MPESA"
                },
                "timeline": [
                    {
                        "action": "BOOKING_CREATED",
                        "timestamp": "2024-12-03T10:00:00Z"
                    },
                    {
                        "action": "PAYMENT_RECEIVED",
                        "timestamp": "2024-12-03T10:15:00Z"
                    }
                ]
            }
        ]
    }
}
```

## Reports Generation

### Generate System Reports
```http
POST /admin/reports/generate
Authorization: Bearer {admin_token}
Content-Type: application/json
```

Request:
```json
{
    "reportType": "REVENUE",
    "dateRange": {
        "start": "2024-12-01",
        "end": "2024-12-31"
    },
    "filters": {
        "propertyTypes": ["HOTEL", "LODGE"],
        "regions": ["Dar es Salaam", "Zanzibar"],
        "paymentMethods": ["MPESA", "CARD"]
    },
    "format": "PDF"
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "report": {
            "id": "report-uuid-1",
            "type": "REVENUE",
            "generatedAt": "2024-12-03T10:00:00Z",
            "downloadUrl": "https://api.example.com/reports/revenue-dec-2024.pdf",
            "summary": {
                "totalRevenue": 275000000,
                "totalBookings": 1250,
                "averageOccupancy": 75.5,
                "topPerformers": [
                    {
                        "propertyName": "Serena Hotel",
                        "revenue": 50000000
                    }
                ]
            }
        }
    }
}
```

## System Configuration

### Update System Settings
```http
PUT /admin/settings/system
Authorization: Bearer {admin_token}
Content-Type: application/json
```

Request:
```json
{
    "bookingSettings": {
        "maxAdvanceBookingDays": 365,
        "minimumAdvanceBookingHours": 24,
        "autoConfirmationEnabled": true,
        "cancellationGracePeriod": 24
    },
    "paymentSettings": {
        "supportedMethods": ["MPESA", "TIGOPESA", "AIRTELMONEY", "CARD"],
        "defaultCurrency": "TZS",
        "minimumDepositPercentage": 20
    },
    "commissionRates": {
        "standard": 10,
        "premium": 15
    }
}
```

Success Response (200 OK):
```json
{
    "status": "success",
    "data": {
        "settings": {
            "updated": [
                "bookingSettings",
                "paymentSettings",
                "commissionRates"
            ],
            "updatedAt": "2024-12-03T10:00:00Z",
            "updatedBy": "admin-uuid-1"
        }
    }
}
```
