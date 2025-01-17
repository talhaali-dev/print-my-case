### **PrintMyCase**
_Prepared by: Talha Ali_ _Date: 1/16/2025_

---

### **1. Executive Summary**
This document outlines the technical requirements for the Print My Case project, a print-on-demand marketplace for phone cases. The platform allows users to create custom designs by uploading features, adding text, changing fonts, and colors. Once finalizing their designs, users can order phone cases for their specific devices. We will implement Stripe for payments, ShipEngine for shipments, and Clerk for user authentication. Sanity CMS will be used for order management, asset management, and database-related tasks.

---

### **2. Scope**
Print My Case is a POD (Print On Demand) platform for phone covers.

#### **2.1 Key Deliverables**
- **Design**: A sleek, modern design that is user-friendly and simple to navigate.
- **Frontend**: A responsive frontend built with React.js/Next.js, TailwindCSS, ShadCN UI, Framer Motion, Fabric.js, React Hook Form, TanStack Query, and TypeScript.
- **Backend**: A scalable backend powered by Sanity CMS, Next.js API routes, Stripe for payments, ShipEngine for shipping, and Clerk for authentication. The backend will ensure seamless interaction between the front end, CMS, and external APIs.
- **Features**: Implementation of user authentication (via Clerk), email notifications (using SendGrid), and payment processing with Stripe. Integration of ShipEngine for real-time shipping quotes and order tracking.
---

### **3. Functional Requirements**
#### **3.1 User Roles**
- **Customer**: End-users who browse, customize, and order phone cases.
- **Seller**: Users who supply designs and manage inventory.
#### **3.2 Features**
- **Customer Features**: 
    - Browse/search products by device model or category.
    - Customize cases with text, images, and colors via a design canvas.
    - Add products to the cart, proceed to checkout, and track orders.
    - View order history and receive notifications.
    - **Email Resend Feature**: Customers can request to resend order confirmation or shipping status emails if they haven't received them.
- **Seller Features**: 
    - Add and manage product templates.
    - Track sales and view analytics dashboards.
    - Access customer feedback.
    - **Email Resend Feature**: Sellers can request to resend order updates to customers.
---

### **4. Non-Functional Requirements**
- **Performance**: Response time under 2 seconds for major operations.
- **Scalability**: The system supports up to 10,000 concurrent users initially.
- **Security**: Implement secure authentication (OAuth2) and data encryption (HTTPS, AES-256).
- **Compliance**: Adhere to GDPR, PCI-DSS, and local e-commerce laws.
- **Reliability**: Ensure 99.9% uptime with failover mechanisms.
---

### **5. System Architecture**
#### **5.1 Overview**
A high-level architecture diagram includes:

- **Frontend**: React/Next.js application.
- **Backend**: API endpoints powered by Next.js and Sanity CMS.
- **Database**: Sanity CMS will serve as both the content management system and the primary database for managing orders, assets, and other relevant data.
- **Third-Party Services**: Stripe for payments, ShipEngine for shipping, SendGrid for transactional emails, and Clerk for user authentication.
#### **5.2 Component Descriptions**
- **Frontend**: Fully responsive with real-time design previews.
- **Backend**: Orchestrates API calls and database interactions with Sanity CMS.
- **APIs**: RESTful endpoints for user authentication, order management, and third-party integrations.
- **CMS**: Sanity for dynamic content and inventory management.
---

### **6. API Requirements**
#### **6.1 Key Endpoints**
- **Products**: 
    - `GET /products` : Fetch available products.
    - `GET /products/:id` : Fetch product details.
    - `POST /products` : Add a new product template (Seller only).
- **Orders**: 
    - `POST /orders` : Create a new order.
    - `GET /orders/:id` : Fetch order details.
    - `POST /orders/resend-email` : Resend order confirmation email to the customer.
- **Users**: 
    - `POST /auth/register` : Register a new user.
    - `POST /auth/login` : Authenticate a user.
    - `GET /users/me` : Fetch current user details.
- **Shipping**: 
    - `GET /shipment` : Fetch shipment tracking details via ShipEngine.
- **Email Resend**: 
    - `POST /emails/resend` : Request to resend an email (e.g., order confirmation, shipping details).
    - **Request Payload**: {
  "userId": "string",
  "orderId": "string",
  "emailType": "string" // Values: "orderConfirmation", "shippingUpdate"
}
    - **Response Example**: {
  "status": "success",
  "message": "Email has been resent successfully."
}
#### **6.2 Sample Response for API Calls**
- **Order Details**: {
  "orderId": 123,
  "status": "In Transit",
  "ETA": "2 days"
}
---

### **7. Data Schema**
#### **Core Entities**
- **Users**:
    - `id` : Unique identifier.
    - `name` : Full name.
    - `email` : User email (unique).
    - `password` : Hashed password.
    - `role` : Customer/Seller.
    - **Stripe Fields**: 
        - `stripe_customer_id` : Stripe customer ID for payment processing.
- **Products**:
    - `id` : Unique identifier.
    - `name` : Product name.
    - `price` : Base price.
    - `description` : Text description.
    - `image` : Default design template.
    - `created_at` , `updated_at` : Timestamps.
    - **ShipEngine Fields**: 
        - `weight` : Product weight for shipping calculations.
        - `dimensions` : Product dimensions (L x W x H).
- **Orders**:
    - `id` : Unique identifier.
    - `user_id` : Linked user ID.
    - `status` : Order status (Pending/Completed).
    - `total_amount` : Final cost.
    - `shipping_address` : Shipping address details.
    - `payment_status` : Stripe payment status.
    - `shipping_status` : ShipEngine status.
- **Custom Designs**:
    - `id` : Unique identifier.
    - `user_id` : Linked user ID.
    - `product_id` : Linked product ID.
    - `design_url` : URL to the custom design.
    - `customization_details` : JSON object with design specifics.
---

### **8. Third-Party Integrations**
- **Stripe**: Secure payment processing. Integrate for handling payments and generating invoices.
- **ShipEngine**: Real-time shipping quotes and tracking integration.
- **Cloud Storage**: Store uploaded assets and designs.
- **Email Notifications (SendGrid)**: Use SendGrid for sending order confirmations, shipping updates, and account-related emails.
- **Clerk for Authentication**: Handling secure user authentication and registration.
---

### **9. Technology Stack**
- **Frontend**: React.js, Next.js, TailwindCSS, ShadCN UI, Fabric.js, Framer Motion.
- **Backend**: Node.js, Sanity CMS, Next.js API routes.
- **Database**: Sanity CMS for dynamic content and database management (products, orders, assets, and user data).
- **APIs**: REST-based with JSON responses.
---

### **12. Appendices**
#### **12.1 Glossary**
- **POD**: Print on Demand.
- **MVP**: Minimum Viable Product.
- **CMS**: Content Management System.
- **API**: Application Programming Interface.
- **Stripe**: Payment gateway service.
- **ShipEngine**: Shipping API service.
#### **12.2 References**
- Stripe API Documentation: [﻿Stripe API Docs](https://stripe.com/docs) 
- ShipEngine API Documentation: [﻿ShipEngine API Docs](https://www.shipengine.com/docs) 
- Sanity CMS: [﻿Sanity CMS Docs](https://www.sanity.io/) 
- Clerk Authentication: [﻿Clerk API Docs](https://clerk.dev/docs) 

![image](https://github.com/user-attachments/assets/687e75e4-fa69-44c8-be3b-ad61d4906127)
