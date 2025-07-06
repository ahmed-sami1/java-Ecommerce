import java.util.*;

interface ShippableItem {
    String getName();
    double getWeight();
}

class ShippingItem implements ShippableItem {
    private String name;
    private double weight;

    public ShippingItem(String name, double weight) {
        this.name = name;
        this.weight = weight;
    }

    @Override
    public String getName() {
        return name;
    }

    @Override
    public double getWeight() {
        return weight;
    }
}

class Product {
    private String name;
    private double price;
    private int quantity;
    private boolean expired;
    private boolean shippable;
    private double weightKg;

    public Product(String name, double price, int quantity, boolean expired, boolean shippable, double weightKg) {
        this.name = name;
        this.price = price;
        this.quantity = quantity;
        this.expired = expired;
        this.shippable = shippable;
        this.weightKg = weightKg;
    }

    public String getName() { return name; }
    public double getPrice() { return price; }
    public int getQuantity() { return quantity; }
    public boolean isExpired() { return expired; }
    public boolean isShippable() { return shippable; }
    public double getWeightKg() { return weightKg; }

    public void reduceQuantity(int amount) {
        this.quantity -= amount;
    }
}

class Customer {
    private String name;
    private double balance;

    public Customer(String name, double balance) {
        this.name = name;
        this.balance = balance;
    }

    public String getName() { return name; }
    public double getBalance() { return balance; }

    public boolean deduct(double amount) {
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
}

class CartItem {
    private Product product;
    private int quantity;

    public CartItem(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
    }

    public Product getProduct() { return product; }
    public int getQuantity() { return quantity; }
}

class Cart {
    private List<CartItem> items = new ArrayList<>();

    public void addItem(Product product, int quantity) {
        if (product.isExpired()) {
            System.out.println("Error: " + product.getName() + " is expired.");
            return;
        }
        if (product.getQuantity() < quantity) {
            System.out.println("Error: Not enough stock for " + product.getName());
            return;
        }
        items.add(new CartItem(product, quantity));
    }

    public List<CartItem> getItems() {
        return items;
    }

    public boolean isEmpty() {
        return items.isEmpty();
    }
}

public class Main {

    public static double calculateShipping(List<CartItem> items) {
        List<ShippableItem> shippingItems = new ArrayList<>();

        for (CartItem item : items) {
            Product p = item.getProduct();
            if (p.isShippable()) {
                shippingItems.add(new ShippingItem(p.getName(), p.getWeightKg() * item.getQuantity()));
            }
        }

        if (shippingItems.isEmpty()) return 0;

        double totalWeight = 0;
        System.out.println("\n** Shipment notice **");
        for (ShippableItem item : shippingItems) {
            System.out.println(item.getName() + " " + (int)(item.getWeight() * 1000) + "g");
            totalWeight += item.getWeight();
        }
        System.out.printf("Total package weight %.1fkg\n", totalWeight);

        return totalWeight * 30; // Shipping cost: 30 per kg
    }

    public static void checkout(Customer customer, Cart cart) {
        if (cart.isEmpty()) {
            System.out.println("Cart is empty. Cannot proceed to checkout.");
            return;
        }

        double subtotal = 0;
        for (CartItem item : cart.getItems()) {
            subtotal += item.getProduct().getPrice() * item.getQuantity();
        }

        double shippingCost = calculateShipping(cart.getItems());
        double total = subtotal + shippingCost;

        if (!customer.deduct(total)) {
            System.out.println("Insufficient balance.");
            return;
        }

        for (CartItem item : cart.getItems()) {
            item.getProduct().reduceQuantity(item.getQuantity());
        }

        System.out.println("\n** Checkout receipt **");
        for (CartItem item : cart.getItems()) {
            System.out.println(item.getQuantity() + "x " + item.getProduct().getName() + " " + (int)(item.getProduct().getPrice() * item.getQuantity()));
        }
        System.out.println("----------------------");
        System.out.println("Subtotal " + (int)subtotal);
        System.out.println("Shipping " + (int)shippingCost);
        System.out.println("Amount " + (int)total);
        System.out.println("Balance " + (int)customer.getBalance());
    }

    public static void main(String[] args) {
        Product cheese = new Product("Cheese", 100, 5, false, true, 0.2);
        Product biscuits = new Product("Biscuits", 150, 3, false, true, 0.7);
        Product scratchCard = new Product("ScratchCard", 50, 10, false, false, 0);

        Customer customer = new Customer("Ahmed", 1000);
        Cart cart = new Cart();

        cart.addItem(cheese, 2);
        cart.addItem(biscuits, 1);
        cart.addItem(scratchCard, 1);

        checkout(customer, cart);
    }
}
