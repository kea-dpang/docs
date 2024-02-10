```mermaid
erDiagram
    CART {
        long id
        map items
    }
    
    USER {
        long id
        string email
        UserStatus status
    }
    
    USER-DETAIL {
        long id
        long employeeNumber
        LocalDate joinDate
        string name
        string phoneNumber
        string zipCode
        string address
        string detailAddress
    }
    
    USER-WITHDRAWAL {
        long id
        list reason
        string message
        LocalDate withdrawalDate 
    }
    
    WISHLIST {
        long id
        list itemIds
    }

    USER ||--|| CART : has
    USER ||--|| USER-DETAIL : has
    USER ||--|| WISHLIST : has
```
