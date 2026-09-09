# STUDY_MATERIALS

Computer systems : A Programmers view (CSAPP)
https://www.youtube.com/playlist?list=PLyboo2CCDSWnhzzzzDQ3OBPrRiIjl-aIE


C++ STL TEMPLATE PROGRAMMING

Question to check in template
1. Write a template function to print all elements of an array.


template <typename T, std::size_t N>
void printArray(const T (&arr)[N]) {
    for (std::size_t i = 0; i < N; ++i) {
        std::cout << arr[i] << " ";
    }
}

int main() {
    int nums[] = {1, 2, 3, 4, 5};
    printArray(nums); // N is automatically deduced as 5
}

template <typename T, std::size_t N>
class StaticBuffer {
    T data[N]; // N determines the raw array dimension
public:
    std::size_t size() const { return N; }
};

int main() {
    StaticBuffer<int, 10> buf; // Passes type 'int' and value '10'
}


template <typename T, std::size_t N>
void processData(const std::array<T, N>& arr) {
    std::cout << "Array size: " << arr.size() << '\n';
}

int main() {
    std::array<int, 3> myArr = {10, 20, 30};
    processData(myArr);
}

2.Create a template specialization for char* comparison.
#include <iostream>
#include <cstring>
using namespace std;

template<typename T>
bool isEqual(T a, T b)
{
    return a == b;
}

// Specialization for char*
template<>
bool isEqual<char*>(char* a, char* b)
{
    return strcmp(a, b) == 0;
}

int main()
{
    cout << isEqual(10, 10) << endl;

    char a[] = "hello";
    char b[] = "hello";

    cout << isEqual(a, b) << endl;
}


1. Generic class template
template<typename T>
class Storage
{
public:
    void print()
    {
        cout << "Generic Storage" << endl;
    }
};

This works for anything:

Storage<int>
Storage<double>
Storage<string>
Storage<int*>

2. Full specialization

Suppose we want completely different behavior for int:

template<>
class Storage<int>
{
public:
    void print()
    {
        cout << "Integer Storage" << endl;
    }
};

This is full specialization because we specify int completely.

Storage<T>       → generic
Storage<int>     → completely specialized

3. Partial specialization

Now suppose we want special behavior for all pointer types.

We don't know what the pointer points to:

int*
double*
char*
MyClass*

So we write:

template<typename T>
class Storage<T*>
{
public:
    void print()
    {
        cout << "Pointer Storage" << endl;
    }
};

Notice this:

Storage<T*>

We have specified the pattern T*, but T is still unknown.

That's why it's called partial specialization.

Storage<T>       → generic
Storage<int>     → full specialization
Storage<T*>      → partial specialization
                    ↑
                    T is still a parameter