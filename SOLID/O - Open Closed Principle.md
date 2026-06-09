Your class should be:
- **Open for extension** → You can add new behavior
- **Closed for modification** → You don't change existing, working code
The goal is to **add new features without touching existing, tested code**. Every time you modify old code, you risk introducing bugs.
# Bad Example
```Java
public class DiscountService {

    public double calculateDiscount(String customerType, double price) {
        if (customerType.equals("REGULAR")) {
            return price * 0.05;
        } else if (customerType.equals("PREMIUM")) {
            return price * 0.10;
        } else if (customerType.equals("VIP")) {
            return price * 0.20;
        }
        // What happens when you need a new "STUDENT" type?
        // You have to come back and modify this method! ❌
        return 0;
    }
}
```
# Good Example
```Java
// Define a common contract (abstraction)
public interface DiscountStrategy {    
	double calculate(double price);
}
```

```Java
// Each customer type is its own class
public class RegularDiscount implements DiscountStrategy {    
	@Override    
	public double calculate(double price) {        
		return price * 0.05;    
	}
}
```

```Java
public class PremiumDiscount implements DiscountStrategy {    
	@Override    
	public double calculate(double price) {        
		return price * 0.10;    
	}
}
```

```Java
public class VIPDiscount implements DiscountStrategy {    
	@Override    
	public double calculate(double price) {        
		return price * 0.20;    
	}
}
```

```Java
// DiscountService never changes when you add new types
public class DiscountService {    
	public double calculateDiscount(DiscountStrategy strategy, double price) {
		return strategy.calculate(price);    
	}
}
```

Now if you need a `StudentDiscount`:

```Java
// Just add a new class — don't touch DiscountService! ✅
public class StudentDiscount implements DiscountStrategy {    
	@Override    
	public double calculate(double price) {        
		return price * 0.15;    
	}
}
```