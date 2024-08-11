# Activity-6
#include <iostream>

class Date {
private:
    int day;
    int month;
    int year;

public:
    Date(int d = 1, int m = 1, int y = 1970) : day(d), month(m), year(y) {}

    // Overload relational operators
    bool operator<(const Date& other) const {
        if (year < other.year) return true;
        if (year == other.year && month < other.month) return true;
        if (year == other.year && month == other.month && day < other.day) return true;
        return false;
    }

    bool operator<=(const Date& other) const {
        return *this < other || *this == other;
    }

    bool operator>(const Date& other) const {
        return !(*this <= other);
    }

    bool operator>=(const Date& other) const {
        return !(*this < other);
    }

    bool operator==(const Date& other) const {
        return day == other.day && month == other.month && year == other.year;
    }

    bool operator!=(const Date& other) const {
        return !(*this == other);
    }

    // Overload ++ operator to increment a date by one day
    Date& operator++() {
        day++;
        if (day > daysInMonth(month, year)) {
            day = 1;
            month++;
            if (month > 12) {
                month = 1;
                year++;
            }
        }
        return *this;
    }

    // Overload + operator to add given number of days to find the next date
    Date operator+(int days) const {
        Date result = *this;
        for (int i = 0; i < days; ++i) {
            result.day++;
            if (result.day > daysInMonth(result.month, result.year)) {
                result.day = 1;
                result.month++;
                if (result.month > 12) {
                    result.month = 1;
                    result.year++;
                }
            }
        }
        return result;
    }

    // Conversion from derived type to basic type (int)
    operator int() const {
        int days = 0;
        for (int m = 1; m < month; ++m) {
            days += daysInMonth(m, year);
        }
        days += day;
        return days;
    }

private:
    int daysInMonth(int m, int y) const {
        if (m == 2) {
            if (isLeapYear(y)) return 29;
            else return 28;
        } else if (m == 4 || m == 6 || m == 9 || m == 11) {
            return 30;
        } else {
            return 31;
        }
    }

    bool isLeapYear(int y) const {
        return (y % 4 == 0 && y % 100 != 0) || y % 400 == 0;
    }
};

int main() {
    Date dt(15, 3, 2022);
    int days = dt; // Assign the number of days elapsed in the current year to the variable days
    std::cout << "Days: " << days << std::endl;

    Date dt2 = dt + 10; // Add 10 days to the current date
    std::cout << "Next date: " << dt2.day << "/" << dt2.month << "/" << dt2.year << std::endl;

    return 0;
}
