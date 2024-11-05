# AMAN-RANA
flight reservation
#include <iostream>
#include <vector>
#include <string>
#include <limits>

using namespace std;

class Flight {
public:
    int flightNumber;
    string destination;
    bool isAvailable;

    Flight(int num, string dest) : flightNumber(num), destination(dest), isAvailable(true) {}
};

class ReservationSystem {
private:
    vector<Flight> flights;

public:
    void addFlight(int flightNumber, string destination) {
        // Check for duplicate flight number
        for (const auto &flight : flights) {
            if (flight.flightNumber == flightNumber) {
                cout << "Flight number " << flightNumber << " already exists.\n";
                return;
            }
        }
        flights.push_back(Flight(flightNumber, destination));
        cout << "Flight " << flightNumber << " to " << destination << " added successfully.\n";
    }

    void bookFlight(int flightNumber) {
        for (auto &flight : flights) {
            if (flight.flightNumber == flightNumber) {
                if (flight.isAvailable) {
                    flight.isAvailable = false;
                    cout << "Flight " << flightNumber << " to " << flight.destination << " booked successfully.\n";
                } else {
                    cout << "Flight " << flightNumber << " is already booked.\n";
                }
                return;
            }
        }
        cout << "Flight " << flightNumber << " not found.\n";
    }

    void cancelBooking(int flightNumber) {
        for (auto &flight : flights) {
            if (flight.flightNumber == flightNumber) {
                if (!flight.isAvailable) {
                    flight.isAvailable = true;
                    cout << "Booking for flight " << flightNumber << " to " << flight.destination << " cancelled successfully.\n";
                } else {
                    cout << "Flight " << flightNumber << " is not booked.\n";
                }
                return;
            }
        }
        cout << "Flight " << flightNumber << " not found.\n";
    }

    void searchFlight(int flightNumber) {
        for (const auto &flight : flights) {
            if (flight.flightNumber == flightNumber) {
                cout << "Flight " << flightNumber << " to " << flight.destination
                     << (flight.isAvailable ? " is available.\n" : " is booked.\n");
                return;
            }
        }
        cout << "Flight " << flightNumber << " not found.\n";
    }

    void listAvailableFlights() {
        cout << "Available Flights:\n";
        bool hasAvailable = false;
        for (const auto &flight : flights) {
            if (flight.isAvailable) {
                cout << "Flight " << flight.flightNumber << " to " << flight.destination << "\n";
                hasAvailable = true;
            }
        }
        if (!hasAvailable) {
            cout << "No available flights.\n";
        }
    }

    void listBookedFlights() {
        cout << "Booked Flights:\n";
        bool hasBooked = false;
        for (const auto &flight : flights) {
            if (!flight.isAvailable) {
                cout << "Flight " << flight.flightNumber << " to " << flight.destination << "\n";
                hasBooked = true;
            }
        }
        if (!hasBooked) {
            cout << "No booked flights.\n";
        }
    }
};

int main() {
    ReservationSystem system;
    system.addFlight(101, "New York");
    system.addFlight(102, "Los Angeles");
    system.addFlight(103, "Chicago");

    int choice, flightNumber;
    string destination;

    while (true) {
        cout << "\nAirline Reservation System\n";
        cout << "1. Book Flight\n";
        cout << "2. Cancel Booking\n";
        cout << "3. Search Flight\n";
        cout << "4. List Available Flights\n";
        cout << "5. List Booked Flights\n";
        cout << "6. Add Flight\n";
        cout << "7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        // Validate choice input
        if (cin.fail() || choice < 1 || choice > 7) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid choice. Please try again.\n";
            continue;
        }

        switch (choice) {
            case 1:
                cout << "Enter flight number to book: ";
                cin >> flightNumber;
                if (cin.fail()) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid flight number. Please try again.\n";
                    continue;
                }
                system.bookFlight(flightNumber);
                break;

            case 2:
                cout << "Enter flight number to cancel booking: ";
                cin >> flightNumber;
                if (cin.fail()) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid flight number. Please try again.\n";
                    continue;
                }
                system.cancelBooking(flightNumber);
                break;

            case 3:
                cout << "Enter flight number to search: ";
                cin >> flightNumber;
                if (cin.fail()) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid flight number. Please try again.\n";
                    continue;
                }
                system.searchFlight(flightNumber);
                break;

            case 4:
                system.listAvailableFlights();
                break;

            case 5:
                system.listBookedFlights();
                break;

            case 6:
                cout << "Enter flight number: ";
                cin >> flightNumber;
                cout << "Enter destination: ";
                cin.ignore(); // Clear the newline from input buffer
                getline(cin, destination);
                system.addFlight(flightNumber, destination);
                break;

            case 7:
                return 0;

            default:
                cout << "Invalid choice. Please try again.\n";
        }
    }
}
