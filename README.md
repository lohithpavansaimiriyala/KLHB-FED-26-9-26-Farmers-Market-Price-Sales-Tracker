# KLHB-FED-26-9-26-Farmers-Market-Price-Sales-Trackerpublic Crop(String name, double pricePerKg, double quantityInStock) {
    this.name = name;
    this.pricePerKg = pricePerKg;
    this.quantityInStock = quantityInStock;
    this.quantitySold = 0;
}

public String getName() {
    return name;
}

public double getPricePerKg() {
    return pricePerKg;
}

public void setPricePerKg(double pricePerKg) {
    this.pricePerKg = pricePerKg;
}

public double getQuantityInStock() {
    return quantityInStock;
}

public double getQuantitySold() {
    return quantitySold;
}


public boolean recordSale(double amount) {
    if (amount <= 0) {
        System.out.println("Invalid quantity! Amount must be greater than zero.");
        return false;
    }
    if (amount > quantityInStock) {
        System.out.println("Not enough stock available! Current stock: " + quantityInStock + " kg");
        return false;
    }
    this.quantitySold += amount;
    this.quantityInStock -= amount;
    return true;
}


public double getTotalSaleAmount() {
    return quantitySold * pricePerKg;
}

public void displayDetails() {
    System.out.println("----------------------------------------");
    System.out.println("Crop Name       : " + name);
    System.out.println("Market Price    : ₹" + pricePerKg + " / kg");
    System.out.println("Current Stock   : " + quantityInStock + " kg");
    System.out.println("Quantity Sold   : " + quantitySold + " kg");
    System.out.println("Total Sales     : ₹" + getTotalSaleAmount());
    System.out.println("----------------------------------------");
}
}

public class FarmersMarketTracker { private static final ArrayList crops = new ArrayList<>(); private static final Scanner scanner = new Scanner(System.in);

public static void main(String[] args) {
    boolean exit = false;

    System.out.println("==============================================");
    System.out.println("  Farmer's Market Price & Sales Tracker");
    System.out.println("==============================================");

    
    while (!exit) {
        printMenu();
        System.out.print("Select an option (1-6): ");
        int choice = getIntInput();

        switch (choice) {
            case 1:
                addCrop();
                break;
            case 2:
                updateMarketPrice();
                break;
            case 3:
                recordSale();
                break;
            case 4:
                viewAllRecords();
                break;
            case 5:
                calculateTotalMarketRevenue();
                break;
            case 6:
                exit = true;
                System.out.println("\nThank you for using the Farmer's Market Tracker!");
                break;
            default:
                System.out.println("Invalid option. Please select a number between 1 and 6.");
        }
    }
    scanner.close();
}

private static void printMenu() {
    System.out.println("\n--- MAIN MENU ---");
    System.out.println("1. Add New Crop Record");
    System.out.println("2. Update Market Price");
    System.out.println("3. Record Crop Sale");
    System.out.println("4. View All Saved Records");
    System.out.println("5. Calculate Total Overall Revenue");
    System.out.println("6. Exit");
}


private static void addCrop() {
    System.out.print("\nEnter Crop Name: ");
    String name = scanner.nextLine().trim();

    if (findCrop(name) != null) {
        System.out.println("A crop with this name already exists in records.");
        return;
[21-09-2026 01:35 PM] darshan: }

    System.out.print("Enter Market Price per kg (₹): ");
    double price = getDoubleInput();

    System.out.print("Enter Available Quantity (kg): ");
    double quantity = getDoubleInput();

    crops.add(new Crop(name, price, quantity));
    System.out.println("Crop '" + name + "' added successfully!");
}


private static void updateMarketPrice() {
    if (crops.isEmpty()) {
        System.out.println("\nNo records available to update.");
        return;
    }

    System.out.print("\nEnter Crop Name to update price: ");
    String name = scanner.nextLine().trim();
    Crop crop = findCrop(name);

    if (crop != null) {
        System.out.println("Current Price: ₹" + crop.getPricePerKg() + " / kg");
        System.out.print("Enter New Market Price per kg (₹): ");
        double newPrice = getDoubleInput();
        crop.setPricePerKg(newPrice);
        System.out.println("Price updated successfully for " + crop.getName() + "!");
    } else {
        System.out.println("Crop not found.");
    }
}


private static void recordSale() {
    if (crops.isEmpty()) {
        System.out.println("\nNo records available.");
        return;
    }

    System.out.print("\nEnter Crop Name for sale: ");
    String name = scanner.nextLine().trim();
    Crop crop = findCrop(name);

    if (crop != null) {
        System.out.print("Enter Quantity Sold (kg): ");
        double qtySold = getDoubleInput();
        if (crop.recordSale(qtySold)) {
            System.out.println("Sale recorded! Sale Amount for this transaction: ₹" + (qtySold * crop.getPricePerKg()));
        }
    } else {
        System.out.println("Crop not found.");
    }
}


private static void viewAllRecords() {
    if (crops.isEmpty()) {
        System.out.println("\nNo crop records found.");
        return;
    }

    System.out.println("\n=== ALL SAVED CROP RECORDS ===");
    for (Crop crop : crops) {
        crop.displayDetails();
    }
}


private static void calculateTotalMarketRevenue() {
    if (crops.isEmpty()) {
        System.out.println("\nNo sales data available.");
        return;
    }

    double totalRevenue = 0;
    for (Crop crop : crops) {
        totalRevenue += crop.getTotalSaleAmount();
    }
    System.out.println("\nTotal Sales Revenue across all crops: ₹" + totalRevenue);
}


private static Crop findCrop(String name) {
    for (Crop crop : crops) {
        if (crop.getName().equalsIgnoreCase(name)) {
            return crop;
        }
    }
    return null;
}


private static int getIntInput() {
    while (!scanner.hasNextInt()) {
        System.out.print("Invalid input. Please enter a number: ");
        scanner.next();
    }
    int value = scanner.nextInt();
    scanner.nextLine(); // Clear buffer
    return value;
}


private static double getDoubleInput() {
    while (!scanner.hasNextDouble()) {
        System.out.print("Invalid input. Please enter a valid decimal number: ");
        scanner.next();
    }
    double value = scanner.nextDouble();
    scanner.nextLine(); // Clear buffer
    return value;
}
}
