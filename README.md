#include <iostream>
using namespace std;

int find_movie(string compare, string movie[], float rating[], int size, int max_size) {
    bool found = false;

    for (int i = 0; i < size; i++) {
        if (compare == movie[i]) {
            found = true;
            cout << "Movie found!\n";
            cout << movie[i] << ": " << rating[i] << "/10\n";
            return i;
        }
    }

    if (size < max_size) {
        cout << "Movie not found but adding it to the list." << endl;
        movie[size] = compare;
        cout << "Please enter the rating of the movie: ";
        cin >> rating[size];
              if (rating[size] < 0 || rating[size] > 10) {
            cout << "Invalid input! Please enter a number between 0 and 10." << endl;
            cin>> rating[size];
        }
        cout << "The movie '" << compare << "' has been added to the list!" << endl;
        size++;
    }

    char choice;
    cout << "Do you want to see the list? (y/n): ";
    cin >> choice;

    if (choice == 'y' || choice == 'Y') {
        cout << "\nMovies and Ratings:\n";
        for (int i = 0; i < size; i++) {
            cout << movie[i] << ": " << rating[i] << endl;
        }
    }
    return -1;
}

int show(string movie[], float rating[], int size, int max_size) {  
cout << "Do you want to see the whole list of movies (y) or search for a movie (s) or none? (y/s/n): ";
    char list;
    cin >> list;

    if (list == 'y' || list == 'Y') {
        cout << "Do you want to sort by (a) Alphabetical order or (r) Rating? (a/r): ";
        char sortChoice;
        cin >> sortChoice;

        if (sortChoice == 'a' || sortChoice == 'A') {
            for (int i = 0; i < size - 1; i++) {
                for (int j = i + 1; j < size; j++) {
                    if (movie[i] > movie[j]) {
                        swap(movie[i], movie[j]);
                        swap(rating[i], rating[j]);
                    }
                }
            }
        } else if (sortChoice == 'r' || sortChoice == 'R') {
            for (int i = 0; i < size - 1; i++) {
                for (int j = i + 1; j < size; j++) {
                    if (rating[i] < rating[j]) {
                        swap(movie[i], movie[j]);
                        swap(rating[i], rating[j]);
                    }
                }
            }
        }

        cout << "The movie list is:\n";
        for (int i = 0; i < size; i++) {
            cout << movie[i] << ": " << rating[i] << endl;
        }
        } else if (list == 's' || list == 'S') {
        string compare;
        cout << "Enter a movie to compare: ";
        cin.ignore();
        getline(cin, compare);
        find_movie(compare, movie, rating, size, max_size);
    } else if (list == 'n' || list == 'N') {  
        cout << "Returning to main menu...\n";
        return 0; 
    } else {
        cout << "Invalid input!\n";
        show(movie, rating, size, max_size);
    }

    return 0;
}
void highest_movie(int size, string movie[], float rating[]) {  
    if (size == 0) {  
        cout << "No movies in the list!" << endl;  
        return;  
    }
    int max_rating = 0; 

    for (int i = 1; i < size; i++) {  
        if (rating[i] > rating[max_rating]) {  
            max_rating = i;
        }
    }

    cout << "Highest-rated movie: " << movie[max_rating] << " with rating " << rating[max_rating] << "/10" << endl;
}
    
   void updateRating(string movie[], float rating[], int size, int max_size) {
    cout << "Enter the name of the movie you would like to change the rating of ";
    cin.ignore();
    string movieName;
    getline(cin, movieName);

    bool found = false;
    for (int i = 0; i < size; i++) {
        if (movie[i] == movieName) {
            float newRating;
            cout << "Enter the new rating (0-10): ";
            cin >> newRating;
            while (newRating < 0 || newRating > 10) {
                cout << "Invalid rating! Please enter a number between 0 and 10: ";
                cin >> newRating;
            }
            rating[i] = newRating;
            cout << "Rating updated successfully!\n";
            show(movie,rating, size, max_size);
            found = true;
        
            
            break;
        }
    }

    if (!found) {
        cout << "Movie not found!" << endl;
    }
}
void deleteMovie(string movie[], float rating[], int size, int max_size) {
    cout << "Do you want to delete any movie from the above movie?  then enter the movie name";
    cin.ignore();
    string movieName;
    getline(cin, movieName);

    bool found = false;
    for (int i = 0; i < size; i++) {
        if (movie[i] == movieName) {
            found = true;
            for (int j = i; j < size - 1; j++) {
                movie[j] = movie[j + 1];  
                rating[j] = rating[j + 1]; 
            }
            size--;
            cout << "Movie deleted successfully!\n";
            show(movie,rating, size, max_size);
            break;
        }
    }

    if (!found) {
        cout << "Movie not found!\n";
    }
}


int main() {
    const int max_size = 10;
     string movie[max_size];
    float rating[max_size];
    int size =5;
   const string movie_name;

    for (int i = 0; i < size; i++) {
        cout << "Enter the movie name: ";
         cin.ignore(); 
        getline(cin, movie[i]);
        cout << "Enter the rating: ";
        cin >> rating[i];
            if (rating[i] < 0 || rating[i] > 10) {
            cout << "Invalid input! Please enter a number between 0 and 10." << endl;
            cin>>rating[i];
            i--;  
        }
    }
 
    show(movie, rating, size, max_size);
        cout << "Do you wanna see the highest-rated movie? (y/n): ";  
    char high;  
    cin >> high;  

    if (high == 'y' || high == 'Y') {  
        highest_movie(size, movie, rating);  
    }
    cout<<"Do you want to update the rating of a movie? (y/n)";
    char update;
    cin>>update;
    if(update == 'y'|| update == 'Y'){
        
        updateRating(movie,rating,5,max_size);
    }if (update != 'y' && update != 'n' && high != 'y' && high != 'n') {
    cout << "Enter y or n: ";
}

deleteMovie( movie,rating,size, max_size);

    return 0;
}
