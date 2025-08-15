import java.util.*;

class Flight {
    private String flightNumber;
    private String source;
    private String destination;
    private int totalSeats;
    private int availableSeats;
    private double price;

    public Flight(String flightNumber, String source, String destination, int totalSeats, double price) {
        this.flightNumber = flightNumber;
        this.source = source;
        this.destination = destination;
        this.totalSeats = totalSeats;
        this.availableSeats = totalSeats;
        this.price = price;
    }

    public String getFlightNumber() {
        return flightNumber;
    }

    public String getSource() {
        return source;
    }

    public String getDestination() {
        return destination;
    }

    public int getAvailableSeats() {
        return availableSeats;
    }

    public double getPrice() {
        return price;
    }

    public boolean bookSeats(int seats) {
        if (seats <= availableSeats) {
            availableSeats -= seats;
            return true;
        }
        return false;
    }

    public void cancelSeats(int seats) {
        availableSeats += seats;
        if (availableSeats > totalSeats) {
            availableSeats = totalSeats; // Prevent over-cancellation
        }
    }

    public void displayDetails() {
        System.out.printf("Flight: %s | %s -> %s | Price: ₹%.2f | Available Seats: %d%n",
                flightNumber, source, destination, price, availableSeats);
    }
}

class Booking {
    private String passengerName;
    private String flightNumber;
    private int seatsBooked;

    public Booking(String passengerName, String flightNumber, int seatsBooked) {
        this.passengerName = passengerName;
        this.flightNumber = flightNumber;
        this.seatsBooked = seatsBooked;
    }

    public String getPassengerName() {
        return passengerName;
    }

    public String getFlightNumber() {
        return flightNumber;
    }

    public int getSeatsBooked() {
        return seatsBooked;
    }
}

public class FlightBookingSystem {
    static Scanner sc = new Scanner(System.in);
    static List<Flight> flights = new ArrayList<>();
    static List<Booking> bookings = new ArrayList<>();

    public static void main(String[] args) {
        initializeFlights();

        int choice;
        do {
            System.out.println("\n=== Flight Ticket Booking System ===");
            System.out.println("1. View Available Flights");
            System.out.println("2. Book Tickets");
            System.out.println("3. Cancel Booking");
            System.out.println("4. View All Bookings");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();
            sc.nextLine(); 

            switch (choice) {
                case 1:
                    viewFlights();
                    break;
                case 2:
                    bookTickets();
                    break;
                case 3:
                    cancelBooking();
                    break;
                case 4:
                    viewBookings();
                    break;
                case 5:
                    System.out.println("Thank you for using the system!");
                    break;
                default:
                    System.out.println("Invalid choice. Try again.");
                    break;
            }
        } while (choice != 5);
    }

    private static void initializeFlights() {
        flights.add(new Flight("AI101", "Delhi", "Mumbai", 50, 4500));
        flights.add(new Flight("AI202", "Chennai", "Bangalore", 30, 2500));
        flights.add(new Flight("AI303", "Kolkata", "Delhi", 40, 4000));
    }

    private static void viewFlights() {
        System.out.println("\nAvailable Flights:");
        for (Flight flight : flights) {
            flight.displayDetails();
        }
    }

    private static void bookTickets() {
        System.out.print("Enter passenger name: ");
        String name = sc.nextLine();

        System.out.print("Enter flight number: ");
        String flightNum = sc.nextLine();

        Flight selectedFlight = findFlight(flightNum);
        if (selectedFlight == null) {
            System.out.println("Flight not found.");
            return;
        }

        System.out.print("Enter number of seats: ");
        int seats = sc.nextInt();
        sc.nextLine();

        if (selectedFlight.bookSeats(seats)) {
            bookings.add(new Booking(name, flightNum, seats));
            System.out.println("Booking successful for " + name);
        } else {
            System.out.println("Not enough seats available.");
        }
    }

    private static void cancelBooking() {
        System.out.print("Enter passenger name to cancel booking: ");
        String name = sc.nextLine();

        Booking bookingToCancel = null;
        for (Booking booking : bookings) {
            if (booking.getPassengerName().equalsIgnoreCase(name)) {
                bookingToCancel = booking;
                break;
            }
        }

        if (bookingToCancel != null) {
            Flight flight = findFlight(bookingToCancel.getFlightNumber());
            if (flight != null) {
                flight.cancelSeats(bookingToCancel.getSeatsBooked());
            }
            bookings.remove(bookingToCancel);
            System.out.println("Booking canceled for " + name);
        } else {
            System.out.println("No booking found for " + name);
        }
    }

    private static void viewBookings() {
        System.out.println("\nAll Bookings:");
        for (Booking booking : bookings) {
            System.out.printf("Passenger: %s | Flight: %s | Seats: %d%n",
                    booking.getPassengerName(), booking.getFlightNumber(), booking.getSeatsBooked());
        }
    }

    private static Flight findFlight(String flightNum) {
        for (Flight flight : flights) {
            if (flight.getFlightNumber().equalsIgnoreCase(flightNum)) {
                return flight;
            }
        }
        return null;
    }
}
