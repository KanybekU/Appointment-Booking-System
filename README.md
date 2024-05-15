
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <ctime>

using namespace std;

// Define structure for appointment
struct Appointment {
    string date;
    string time;
    string service;
    string professional;
};

// Function to add appointment
void createAppointment(vector<Appointment> &appointments) {
    Appointment newAppointment;
    // Get input for new appointment
    cout << "Enter date (YYYY-MM-DD): ";
    cin >> newAppointment.date;
    cout << "Enter time (HH:MM): ";
    cin >> newAppointment.time;
    cin.ignore(); // Ignore newline character
    cout << "Enter service: ";
    getline(cin, newAppointment.service);
    cout << "Enter professional: ";
    getline(cin, newAppointment.professional);
    // Add to appointments vector
    appointments.push_back(newAppointment);
}

// Function to display all appointments
void displayAppointments(const vector<Appointment> &appointments) {
    cout << "Appointments:\n";
    for(const auto& appointment : appointments) {
        cout << "Date: " << appointment.date 
             << ", Time: " << appointment.time
             << ", Service: " << appointment.service 
             << ", Professional: " << appointment.professional << endl;
    }
}

// Function to save appointments to file
void saveAppointments(const vector<Appointment> &appointments, const string& filename) {
    ofstream file(filename);
    if (file.is_open()) {
        for(const auto& appointment : appointments) {
            file << appointment.date << " " << appointment.time << " "
                 << appointment.service << " " << appointment.professional << endl;
        }
        file.close();
    } else {
        cerr << "Unable to open file for writing.";
    }
}

// Function to load appointments from file
void loadAppointments(vector<Appointment> &appointments, const string& filename) {
    ifstream file(filename);
    if (file.is_open()) {
        appointments.clear(); // Clear existing appointments
        Appointment appointment;
        while (file >> appointment.date >> appointment.time) {
            getline(file >> ws, appointment.service); // Read service with spaces
            getline(file >> ws, appointment.professional); // Read professional with spaces
            appointments.push_back(appointment);
        }
        file.close();
    } else {
        cerr << "Unable to open file for reading.";
    }
}

int main() {
    vector<Appointment> appointments;
    string filename = "appointments.txt"; // File to store appointments

    // Load appointments from file
    loadAppointments(appointments, filename);

    int choice;
    do {
        // Display menu
        cout << "\nAppointment Booking System\n";
        cout << "1. Add Appointment\n";
        cout << "2. View Appointments\n";
        cout << "3. Save Appointments\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                createAppointment(appointments);
                break;
            case 2:
                displayAppointments(appointments);
                break;
            case 3:
                saveAppointments(appointments, filename);
                break;
            case 4:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while(choice != 4);

    return 0;
}

