# ALX Travel App 0x01

A Django REST API for a travel booking application that allows users to list properties, make bookings, and leave reviews. This version includes comprehensive API endpoints with full CRUD operations.

## Project Structure

This is the `alx_travel_app_0x01` version, duplicated from `alx_travel_app_0x00` with enhanced API functionality.

## Features

- **Listings Management**: Create, view, update, and delete travel property listings
- **Booking System**: Users can book properties with date validation and capacity checks
- **Review System**: Guests can leave reviews and ratings for properties they've stayed at
- **Image Upload**: Support for multiple images per listing
- **Advanced Filtering**: Filter listings by type, location, price range, and guest capacity
- **Search Functionality**: Search listings by title, description, or location
- **User Authentication**: JWT-based authentication for secure API access
- **RESTful API**: Full CRUD operations via Django REST Framework ViewSets
- **API Documentation**: Swagger/OpenAPI documentation

## API Endpoints

All endpoints are accessible under `/api/` and follow RESTful conventions:

### Listings (`/api/listings/`)
- `GET /api/listings/` - List all active listings with filtering and search
- `POST /api/listings/` - Create a new listing (authenticated users)
- `GET /api/listings/{id}/` - Get listing details
- `PUT /api/listings/{id}/` - Update entire listing (owner only)
- `PATCH /api/listings/{id}/` - Partially update listing (owner only)
- `DELETE /api/listings/{id}/` - Delete listing (owner only)

#### Custom Actions:
- `GET /api/listings/{id}/reviews/` - Get all reviews for a listing
- `GET /api/listings/{id}/bookings/` - Get all bookings for a listing (owner only)

### Bookings (`/api/bookings/`)
- `GET /api/bookings/` - List user's bookings (as guest or listing owner)
- `POST /api/bookings/` - Create a new booking
- `GET /api/bookings/{id}/` - Get booking details
- `PUT /api/bookings/{id}/` - Update entire booking
- `PATCH /api/bookings/{id}/` - Partially update booking
- `DELETE /api/bookings/{id}/` - Cancel booking

#### Custom Actions:
- `PATCH /api/bookings/{id}/update_status/` - Update booking status (owner only)

### Reviews (`/api/reviews/`)
- `GET /api/reviews/` - List all reviews (filterable by listing)
- `POST /api/reviews/` - Create a new review
- `GET /api/reviews/{id}/` - Get review details
- `PUT /api/reviews/{id}/` - Update entire review (author only)
- `PATCH /api/reviews/{id}/` - Partially update review (author only)
- `DELETE /api/reviews/{id}/` - Delete review (author only)

### Listing Images (`/api/listing-images/`)
- `GET /api/listing-images/` - List all images
- `POST /api/listing-images/` - Upload new image
- `GET /api/listing-images/{id}/` - Get image details
- `PUT /api/listing-images/{id}/` - Update image
- `PATCH /api/listing-images/{id}/` - Partially update image
- `DELETE /api/listing-images/{id}/` - Delete image

## Query Parameters

### Listings
- `type` - Filter by listing type (hotel, apartment, villa, resort, hostel, other)
- `location` - Filter by location (case-insensitive partial match)
- `min_price` - Minimum price per night
- `max_price` - Maximum price per night
- `guests` - Minimum guest capacity
- `search` - Search in title, description, or location

### Reviews
- `listing` - Filter reviews by listing ID

## Setup Instructions

### Prerequisites
- Python 3.8+ (tested with Python 3.12.4)
- pip (Python package installer)
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd alx_travel_app_0x01
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   
   # On Linux/Mac:
   source venv/bin/activate
   
   # On Windows:
   venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure settings**
   Make sure your `alx_travel_app/settings.py` includes the listings app and DRF configuration:
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'rest_framework',
       'drf_yasg',  # For Swagger documentation
       'listings',
   ]
   
   REST_FRAMEWORK = {
       'DEFAULT_PERMISSION_CLASSES': [
           'rest_framework.permissions.IsAuthenticatedOrReadOnly',
       ],
       'DEFAULT_AUTHENTICATION_CLASSES': [
           'rest_framework.authentication.SessionAuthentication',
           'rest_framework.authentication.BasicAuthentication',
       ],
   }
   ```



5. **Run migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create superuser**
   ```bash
   python manage.py createsuperuser
   ```

7. **Seed the database (optional)**
   ```bash
   python manage.py seed
   ```

8. **Run the development server**
   ```bash
   python manage.py runserver
   ```

## Testing the API

### Using Postman

1. **Import the following endpoints for testing:**

   **GET Requests (No authentication required):**
   - `GET http://localhost:8000/api/listings/`
   - `GET http://localhost:8000/api/listings/1/`
   - `GET http://localhost:8000/api/listings/1/reviews/`

   **POST Requests (Authentication required):**
   - `POST http://localhost:8000/api/listings/`
   - `POST http://localhost:8000/api/bookings/`
   - `POST http://localhost:8000/api/reviews/`

   **PUT/PATCH Requests (Owner permissions required):**
   - `PUT http://localhost:8000/api/listings/1/`
   - `PATCH http://localhost:8000/api/bookings/1/`

   **DELETE Requests (Owner permissions required):**
   - `DELETE http://localhost:8000/api/listings/1/`
   - `DELETE http://localhost:8000/api/bookings/1/`

2. **Authentication Setup:**
   - Use Django's session authentication or basic authentication
   - Create a user via admin panel or registration endpoint
   - Include authentication headers in requests

3. **Sample POST Data:**

   **Create Listing:**
   ```json
   {
       "title": "Beautiful Beachfront Villa",
       "description": "A stunning villa with ocean views",
       "location": "Malibu, CA",
       "price_per_night": 250.00,
       "max_guests": 6,
       "listing_type": "villa",
       "amenities": ["WiFi", "Pool", "Ocean View"]
   }
   ```

   **Create Booking:**
   ```json
   {
       "listing": 1,
       "check_in_date": "2024-07-15",
       "check_out_date": "2024-07-20",
       "number_of_guests": 4
   }
   ```

   **Create Review:**
   ```json
   {
       "listing": 1,
       "rating": 5,
       "comment": "Amazing property with great amenities!"
   }
   ```

### Using curl

```bash
# Get all listings
curl -X GET http://localhost:8000/api/listings/

# Get filtered listings
curl -X GET "http://localhost:8000/api/listings/?location=beach&min_price=100"

# Create a listing (with authentication)
curl -X POST http://localhost:8000/api/listings/ \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic <your-auth-token>" \
  -d '{"title": "Test Listing", "description": "Test", "location": "Test City", "price_per_night": 100, "max_guests": 2, "listing_type": "apartment"}'
```

## API Documentation

When running the development server, access API documentation at:
- **Swagger UI**: http://localhost:8000/swagger/
- **ReDoc**: http://localhost:8000/redoc/

## Key Features Implemented

### ViewSets with CRUD Operations
- `ListingViewSet`: Full CRUD with custom filtering and search
- `BookingViewSet`: CRUD with user-specific querysets and status updates
- `ReviewViewSet`: CRUD with ownership validation
- `ListingImageViewSet`: Image upload and management

### Permission System
- `IsAuthenticatedOrReadOnly`: Read access for all, write access for authenticated users
- Owner-only operations: Users can only modify their own content
- Custom permission checks in `perform_update` and `perform_destroy` methods

### Advanced Filtering
- Query parameter-based filtering for listings
- Search functionality across multiple fields
- User-specific data filtering for bookings and reviews

### Data Validation
- Automatic user assignment on creation
- Business logic validation (e.g., booking dates, guest capacity)
- Ownership verification for updates and deletions

## Repository Information

- **GitHub repository**: `alx_travel_app_0x01`
- **Directory**: `alx_travel_app`
- **Key Files**: 
  - `listings/views.py` - API ViewSets implementation
  - `listings/urls.py` - URL routing configuration
  - `README.md` - Project documentation

## Error Handling

The API returns appropriate HTTP status codes:
- `200 OK` - Successful GET, PUT, PATCH
- `201 Created` - Successful POST
- `204 No Content` - Successful DELETE
- `400 Bad Request` - Validation errors
- `401 Unauthorized` - Authentication required
- `403 Forbidden` - Permission denied
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server errors

## Next Steps

1. Test all endpoints thoroughly using Postman or similar tools
2. Implement additional authentication methods (JWT, OAuth)
3. Add pagination for large datasets
4. Implement caching for improved performance
5. Add comprehensive test suite
6. Deploy to production environment
