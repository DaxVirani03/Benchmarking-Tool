#include <iostream>
#include <fstream>
#include <chrono>
#include <cstdio> // Include for remove function
#include <cstdlib> // Include for rand and srand
#include <ctime>   // Include for time
#include <vector>  // Include for vector
using namespace std;

void writeSequentially(long long s, int b_size) {
    ofstream file("ygvecj.txt", ios::binary);
    if (file.is_open()) {
        auto start_loop = chrono::high_resolution_clock::now();
        char* buffer = new char[b_size]; // Create a buffer
        for (long long i = 0; i < s; i += b_size) {
            int bytes_to_write = min(b_size, static_cast<int>(s - i));
            file.write(buffer, bytes_to_write); // Write the buffer
        }
        auto end_loop = chrono::high_resolution_clock::now();
        auto duration_loop = chrono::duration_cast<chrono::milliseconds>(end_loop - start_loop);
        cout << "\nResult for Writing (Sequential): " << (static_cast<double>(s) / (1024 * 1024)) / (duration_loop.count() / 1000.0) << " MBps";
        delete[] buffer; // Free the buffer
        file.close();
    } else {
        cout << "Failed to open the file for writing." << endl;
    }
}

void writeRandomly(long long s, int b_size) {
    ofstream file("ygvecj.txt", ios::binary);
    if (file.is_open()) {
        auto start_loop = chrono::high_resolution_clock::now();
        char* buffer = new char[b_size]; // Create a buffer
        vector<long long> positions; // To store random positions
        long long total_blocks = s / b_size;

        // Generate random positions
        for (long long i = 0; i < total_blocks; ++i) {
            positions.push_back(rand() % total_blocks);
        }

        // Write to random positions
        for (long long pos : positions) {
            file.seekp(pos * b_size); // Move to the random position
            file.write(buffer, b_size); // Write the buffer
        }

        auto end_loop = chrono::high_resolution_clock::now();
        auto duration_loop = chrono::duration_cast<chrono::milliseconds>(end_loop - start_loop);
        cout << "\nResult for Writing (Random): " << (static_cast<double>(s) / (1024 * 1024)) / (duration_loop.count() / 1000.0) << " MBps";
        delete[] buffer; // Free the buffer
        file.close();
    } else {
        cout << "Failed to open the file for writing." << endl;
    }
}

void readFile(int b_size) {
    ifstream file_in("ygvecj.txt", ios::binary);
    if (file_in.is_open()) {
        char* buffer = new char[b_size]; // Create a buffer for reading
        long long total_bytes_read = 0;
        auto start_read = chrono::high_resolution_clock::now();

        while (file_in.read(buffer, b_size)) // Read in chunks of b_size
        {
            total_bytes_read += file_in.gcount(); // Count the number of bytes read
        }

        // Handle any remaining bytes that may not fill the buffer
        total_bytes_read += file_in.gcount(); // Add any remaining bytes
        auto end_read = chrono::high_resolution_clock::now();
        auto duration_read = chrono::duration_cast<chrono::milliseconds>(end_read - start_read);

        cout << "\nResult for Reading: " << (static_cast<double>(total_bytes_read) / (1024 * 1024)) / (duration_read.count() / 1000.0) << " MBps";
        cout << "\nTime taken to read the file: " << duration_read.count() << " ms" << endl;

        delete[] buffer; // Free the buffer
        file_in.close();
    } else {
        cout << "Failed to open the file for reading." << endl;
    }
}

int main() {
    int main_choice;

    cout << "Choose an option:\n";
    cout << "1. Check Real-World Results\n";
       cout << "2. Enter Custom Settings\n";
    cout << "Enter your choice (1 or 2): ";
    cin >> main_choice;

    if (main_choice == 1) {
        int size = 1;
        int batch_size = 1 * 1024 * 1024; // 1 MB

        cout << "Writing 1 GB of data sequentially...\n";
        writeSequentially(size, batch_size);

        cout << "Reading the file...\n";
        readFile(batch_size);

        // Remove the file after the test
        if (remove("ygvecj.txt") != 0) {
            cout << "Error deleting the file." << endl;
        } else {
            cout << "File deleted successfully." << endl;
        }
        return 0; // Exit the program
    } else if (main_choice == 2) {
        double size_gb; // Use double to allow for fractional GB input
        int batch_size;
        int choice;

        cout << "Enter the Size (in gigabytes) to write: ";
        cin >> size_gb;
        long long size_bytes = static_cast<long long>(size_gb * 1073741824); // Convert GB to bytes

        cout << "Enter batch size (bytes to read at once): ";
        cin >> batch_size;

        cout << "Choose the writing method:\n";
        cout << "1. Write Sequentially\n";
        cout << "2. Write Randomly\n";
        cout << "Enter your choice (1 or 2): ";
        cin >> choice;

        // Seed the random number generator
        srand(static_cast<unsigned int>(time(0)));

        if (choice == 1) {
            writeSequentially(size_bytes, batch_size);
        } else if (choice == 2) {
            writeRandomly(size_bytes, batch_size);
        } else {
            cout << "Invalid choice. Please select 1 or 2." << endl;
            return 1; // Exit with an error code
        }

        // Read the file after writing
        readFile(batch_size);

        // Remove the file after the test
        if (remove("ygvecj.txt") != 0) {
            cout << "Error deleting the file." << endl;
        } else {
            cout << "File deleted successfully." << endl;
        }
    } else {
        cout << "Invalid choice. Please select 1 or 2." << endl;
        return 1; // Exit with an error code
    }

    return 0;
}
