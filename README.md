```#include <iostream>
#include <sstream>
#include <utility>
#include <limits>
using namespace std;

void
sift_down( double * elements, unsigned int size, unsigned int current_element_index );

void
build_heap( double * elements, unsigned int size );

void
heap_sort( double * elements , unsigned int size );

static string error_message = "An error has occured while reading input data\n";

int
main ()
{
    unsigned int size;
    if( cin >> size ) {
        double * elements = new double[ size ];
        
        cin.ignore( std::numeric_limits<streamsize>::max(), '\n' );
        string line;
        if( getline( cin, line ) ) {
            istringstream stream( line );
            bool success = true;
            for( unsigned int i = 0; i < size; ++i ) {
                if ( !( stream >> elements[ i ] ) ) {
                    success = false;
                    break;
                }
            }
            success &= stream.eof();
            
            if( success ) {
                heap_sort( elements, size );
                for( unsigned int i = 0; i < size; ++i ) {
                    cout << elements[ i ] << " ";
                }
            }
            else {
                cerr << error_message;
            }
        }
        
        delete [] elements;
    }
    else {
        cerr << error_message;
    }
    
    return 0;
}

void
heap_sort( double * elements, unsigned int size )
{
    build_heap( elements, size );
    
    for( unsigned int i = 0; i < size; ++i ) {
        swap( elements[ 0 ], elements[ size - 1 - i ] );
        sift_down( elements, size - 1 - i, 0 );
    }
}

void
build_heap( double * elements, unsigned int size )
{
    for( unsigned int i = 0; i < size / 2; ++i ) {
        sift_down( elements, size,  size / 2 - 1 - i );
    }
}

void
sift_down( double * elements, unsigned int size, unsigned int current_element_index )
{
    unsigned int left_child_index = 2 * current_element_index + 1;
    unsigned int right_child_index = 2 * current_element_index + 2;
    int largest_element_index = current_element_index;
    
    if( left_child_index < size && elements[ left_child_index ] > elements[ largest_element_index ] ) {
        largest_element_index = left_child_index;
    }
    if( right_child_index < size && elements[ right_child_index ] > elements[ largest_element_index ] ) {
        largest_element_index = right_child_index;
    }
    
    if( largest_element_index != current_element_index )
    {
        swap( elements[ current_element_index ], elements[ largest_element_index ] );
        sift_down( elements, size, largest_element_index );
    }
}
```
