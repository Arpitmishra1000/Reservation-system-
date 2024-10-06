# Reservation-system-

import java.util.*;

class User {
    String username, password;
    User(String username, String password) {
        this.username = username; this.password = password;
    }
}

class Reservation {
    String pnr, name, trainNumber, trainName, classType, dateOfJourney, from, to;
    Reservation(String pnr, String name, String trainNumber, String trainName, 
                String classType, String dateOfJourney, String from, String to) {
        this.pnr = pnr; this.name = name; this.trainNumber = trainNumber; 
        this.trainName = trainName; this.classType = classType; 
        this.dateOfJourney = dateOfJourney; this.from = from; this.to = to;
    }
}

public class OnlineReservationSystem {
    private static final Map<String, User> users = new HashMap<>();
    private static final Map<String, Reservation> reservations = new HashMap<>();
    private static final Scanner scanner = new Scanner(System.in);
    
    public static void main(String[] args) {
        users.put("user1", new User("user1", "password1"));
        if (login()) userOptions();
    }

    private static boolean login() {
        System.out.println("Welcome to the Online Reservation System");
        System.out.print("Username: "); String username = scanner.nextLine();
        System.out.print("Password: "); String password = scanner.nextLine();
        if (users.containsKey(username) && users.get(username).password.equals(password)) {
            System.out.println("Login successful!");
            return true;
        }
        System.out.println("Invalid credentials. Exiting.");
        return false;
    }

    private static void userOptions() {
        while (true) {
            System.out.println("\n1. Make a Reservation\n2. Cancel a Reservation\n3. Exit");
            switch (scanner.nextInt()) {
                case 1 -> makeReservation();
                case 2 -> cancelReservation();
                case 3 -> { System.out.println("Exiting system."); return; }
                default -> System.out.println("Invalid choice. Try again.");
            }
            scanner.nextLine();  // Consume newline
        }
    }

    private static void makeReservation() {
        System.out.print("Enter your name: ");
        String pnr = UUID.randomUUID().toString();
        String name = scanner.nextLine();
        System.out.print("Train number: "); String trainNumber = scanner.nextLine();
        System.out.print("Train name: "); String trainName = scanner.nextLine();
        System.out.print("Class type: "); String classType = scanner.nextLine();
        System.out.print("Date of journey (YYYY-MM-DD): "); String dateOfJourney = scanner.nextLine();
        System.out.print("From (place): "); String from = scanner.nextLine();
        System.out.print("To (destination): "); String to = scanner.nextLine();
        
        reservations.put(pnr, new Reservation(pnr, name, trainNumber, trainName, classType, dateOfJourney, from, to));
        System.out.println("Reservation successful! Your PNR is: " + pnr);
    }

    private static void cancelReservation() {
        System.out.print("Enter your PNR number: ");
        Reservation reservation = reservations.remove(scanner.nextLine());
        if (reservation != null) {
            System.out.println("Reservation cancelled successfully!\nDetails: " +
                "\nName: " + reservation.name +
                "\nTrain Number: " + reservation.trainNumber +
                "\nTrain Name: " + reservation.trainName +
                "\nClass Type: " + reservation.classType +
                "\nDate of Journey: " + reservation.dateOfJourney +
                "\nFrom: " + reservation.from +
                "\nTo: " + reservation.to);
        } else {
            System.out.println("Invalid PNR number. No reservation found.");
        }
    }
}
