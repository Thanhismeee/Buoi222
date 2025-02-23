erDiagram
    CUSTOMER ||--o{ RESERVATION : "makes"
    CUSTOMER {
        int customer_id PK
        string name
        decimal wallet_balance
        string email UK
        string phone
        datetime created_at
    }
    
    MANAGER ||--o{ STAFF : "manages"
    MANAGER {
        int manager_id PK
        string name
        string email UK
        string phone
        datetime created_at
    }
    
    STAFF ||--o{ RESERVATION : "handles"
    STAFF {
        int staff_id PK
        int manager_id FK
        string name
        string role
        string status
        string email UK
        string phone
        datetime created_at
    }
    
    KOI_FARM ||--o{ FISH_ORDER : "supplies"
    KOI_FARM {
        int farm_id PK
        string name
        string location
        string contact_person
        string phone
        string email
        datetime created_at
    }
    
    RESERVATION ||--o{ FISH_ORDER : "contains"
    RESERVATION {
        int reservation_id PK
        int customer_id FK
        int staff_id FK
        date departure_date
        decimal expected_budget
        string status
        decimal deposit_amount
        decimal total_cost
        datetime created_at
        datetime updated_at
    }
    
    RESERVATION ||--o{ PAYMENT : "has"
    FISH_ORDER {
        int order_id PK
        int reservation_id FK
        int farm_id FK
        string breed
        string size
        int age
        decimal price
        date delivery_date
        string status
        datetime created_at
        datetime updated_at
    }
    
    PAYMENT {
        int payment_id PK
        int reservation_id FK
        decimal amount
        string payment_type
        datetime payment_date
        string status
        string transaction_id
        datetime created_at
    }
