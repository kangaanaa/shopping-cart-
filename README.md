import java.util.*;

class Product {
    private String name;
    private double price;
    private int quantity;

    public Product(String name, double price, int quantity) {
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }
    public String getName() { return name; }
    public double getPrice() { return price; }
    public int getQuantity() { return quantity; }
    public double getTotalPrice() { return price * quantity; }
}

abstract class Discount {
    public abstract double apply(double total, ArrayList<Product> products);
}

class FestiveDiscount extends Discount {
    public double apply(double total, ArrayList<Product> products) {
        return total * 0.9;
    }
}

class BulkDiscount extends Discount {
    public double apply(double total, ArrayList<Product> products) {
        for (Product p : products) {
            if (p.getQuantity() > 5) return total * 0.8;
        }
        return total;
    }
}

interface Payment {
    void pay(double amount);
}

class OnlinePayment implements Payment {
    public void pay(double amount) {
        System.out.printf("Total Amount Payable: %.2f%n", amount);
    }
}

public class ShoppingCartGirl {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = Integer.parseInt(sc.nextLine());
        ArrayList<Product> cart = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            String[] input = sc.nextLine().split(" ");
            String name = input[0];
            double price = Double.parseDouble(input[1]);
            int quantity = Integer.parseInt(input[2]);
            cart.add(new Product(name, price, quantity));
        }
        String discountType = sc.nextLine().trim().toLowerCase();
        double total = 0;
        for (Product p : cart) total += p.getTotalPrice();
        Discount discount = discountType.equals("festive") ? new FestiveDiscount() : new BulkDiscount();
        double finalAmount = discount.apply(total, cart);
        for (Product p : cart) {
            System.out.println("Product: " + p.getName() + ", Price: " + p.getPrice() + ", Quantity: " + p.getQuantity());
        }
        Payment payment = new OnlinePayment();
        payment.pay(finalAmount);
        sc.close();
    }
}
