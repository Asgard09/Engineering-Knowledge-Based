If class `B` extends class `A`, then anywhere you use `A`, you should be able to swap in `B` without anything breaking.
# Bad Example
```Java
public class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}
```

```Java
public class Square extends Rectangle {

    // A square must keep width == height
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // forces both dimensions to be equal
    }

    @Override
    public void setHeight(int height) {
        this.width = height; // same here
        this.height = height;
    }
}
```

```Java
public class Main {

    // This method works correctly with Rectangle
    public static void printArea(Rectangle r) {
        r.setWidth(5);
        r.setHeight(10);
        // Expected: 5 * 10 = 50
        System.out.println("Area: " + r.getArea());
    }

    public static void main(String[] args) {
        printArea(new Rectangle()); // Area: 50 ✅
        printArea(new Square());    // Area: 100 ❌ — height overrides width!
    }
}
```
# Good Example
```Java
// Shared abstraction
public interface Shape {
    int getArea();
}
```

```Java
public class Main {

    public static void printArea(Shape shape) {
        System.out.println("Area: " + shape.getArea());
    }

    public static void main(String[] args) {
        printArea(new Rectangle(5, 10)); // Area: 50 ✅
        printArea(new Square(5));        // Area: 25 ✅
    }
}
```
