# Java-individuam-work-cheese-shop

#CHEESE.JAVA

public class Cheese {
    private int id;
    private String name;
    private double price;

    public Cheese(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return id + ": " + name + " - $" + price;
    }
}


#CHEESESHOP

import java.util.ArrayList;
import java.util.List;

public class CheeseShop {
    private List<Cheese> availableCheeses;
    private List<Cheese> cart;

    public CheeseShop() {
        availableCheeses = new ArrayList<>();
        cart = new ArrayList<>();
    }

    // Shop owner methods
    public void addCheese(Cheese cheese) {
        availableCheeses.add(cheese);
        System.out.println(cheese.getName() + " has been added to the shop.");
    }

    public void removeCheese(int id) {
        availableCheeses.removeIf(cheese -> cheese.getId() == id);
        System.out.println("Cheese with id " + id + " has been removed from the shop.");
    }

    // Customer methods
    public void addCheeseToCart(int id) {
        for (Cheese cheese : availableCheeses) {
            if (cheese.getId() == id) {
                cart.add(cheese);
                System.out.println(cheese.getName() + " has been added to your cart.");
                return;
            }
        }
        System.out.println("Cheese with id " + id + " not found in the shop.");
    }

    public void removeCheeseFromCart(int id) {
        cart.removeIf(cheese -> cheese.getId() == id);
        System.out.println("Cheese with id " + id + " has been removed from your cart.");
    }

    public void displayAvailableCheeses() {
        if (availableCheeses.isEmpty()) {
            System.out.println("The shop is empty.");
        } else {
            System.out.println("🧀 Available cheeses in the shop:");
            for (Cheese cheese : availableCheeses) {
                System.out.println(cheese);
            }
        }
    }

    public void displayCart() {
        if (cart.isEmpty()) {
            System.out.println("Your cart is empty.");
        } else {
            System.out.println("🛒 Cheeses in your cart:");
            for (Cheese cheese : cart) {
                System.out.println(cheese);
            }
        }
    }

    public void checkout() {
        if (cart.isEmpty()) {
            System.out.println("Your cart is empty.");
            return;
        }

        double totalPrice = 0;
        System.out.println("🧾 Items in your cart:");
        for (Cheese cheese : cart) {
            System.out.println(cheese);
            totalPrice += cheese.getPrice();
        }
        System.out.println("💰 Total price: $" + totalPrice);
        cart.clear();
    }
}


#MAINJAVA

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        CheeseShop cheeseShop = new CheeseShop();
        Scanner scanner = new Scanner(System.in);

        // Adding some initial cheeses to the shop
        cheeseShop.addCheese(new Cheese(1, "Gorgonzola", 18.99));
        cheeseShop.addCheese(new Cheese(2, "Manchego", 20.49));
        cheeseShop.addCheese(new Cheese(3, "Roquefort", 22.99));
        cheeseShop.addCheese(new Cheese(4, "Pecorino Romano", 25.99));
        cheeseShop.addCheese(new Cheese(5, "Comté", 27.49));

        System.out.println("🎉 Welcome to the Exquisite Cheese Shop! 🎉");
        System.out.println("Hello! What would you like to by?");
        System.out.println("These are the cheeses we have in stock:");
        cheeseShop.displayAvailableCheeses();
        
        while (true) {
            System.out.println("\nAvailable commands:");
            System.out.println("1. add [id] [cheese name] [price]");
            System.out.println("2. remove [id]");
            System.out.println("3. addtocart [id]");
            System.out.println("4. removefromcart [id]");
            System.out.println("5. checkout");
            System.out.println("6. viewshop");
            System.out.println("7. viewcart");
            System.out.println("8. exit");
            System.out.print("Enter command: ");

            String command = scanner.nextLine();
            String[] parts = command.split(" ", 2);
            String action = parts[0];

            if (action.equalsIgnoreCase("add") && parts.length == 2) {
                String[] params = parts[1].split(" ");
                int id = Integer.parseInt(params[0]);
                String cheeseName = params[1];
                double price = Double.parseDouble(params[2]);
                cheeseShop.addCheese(new Cheese(id, cheeseName, price));
            } else if (action.equalsIgnoreCase("remove") && parts.length == 2) {
                int id = Integer.parseInt(parts[1]);
                cheeseShop.removeCheese(id);
            } else if (action.equalsIgnoreCase("addtocart") && parts.length == 2) {
                int id = Integer.parseInt(parts[1]);
                cheeseShop.addCheeseToCart(id);
            } else if (action.equalsIgnoreCase("removefromcart") && parts.length == 2) {
                int id = Integer.parseInt(parts[1]);
                cheeseShop.removeCheeseFromCart(id);
            } else if (action.equalsIgnoreCase("checkout")) {
                cheeseShop.checkout();
            } else if (action.equalsIgnoreCase("viewshop")) {
                cheeseShop.displayAvailableCheeses();
            } else if (action.equalsIgnoreCase("viewcart")) {
                cheeseShop.displayCart();
            } else if (action.equalsIgnoreCase("exit")) {
                System.out.println("Thank you for visiting the Exquisite Cheese Shop! Have a cheesy day! 🧀");
                break;
            } else {
                System.out.println("Invalid command. Please try again.");
            }
        }

        scanner.close();
    }
}

