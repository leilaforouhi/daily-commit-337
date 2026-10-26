from statistics import mean


def analyze_expenses(daily_expenses, threshold=1.5):
    if not daily_expenses:
        raise ValueError("Expense records cannot be empty")

    if threshold <= 0:
        raise ValueError("Threshold must be positive")

    if any(amount < 0 for amount in daily_expenses):
        raise ValueError("Expenses cannot be negative")

    average = mean(daily_expenses)
    alerts = []

    for day, amount in enumerate(daily_expenses, start=1):
        if average > 0 and amount > average * threshold:
            alerts.append({
                "day": day,
                "amount": round(amount, 2),
                "average": round(average, 2),
                "difference": round(amount - average, 2),
            })

    return {
        "days_analyzed": len(daily_expenses),
        "average_daily_expense": round(average, 2),
        "highest_expense": round(max(daily_expenses), 2),
        "total_expenses": round(sum(daily_expenses), 2),
        "alerts": alerts,
    }


if __name__ == "__main__":
    try:
        raw = input("Enter daily expenses separated by spaces: ")
        expenses = [float(value) for value in raw.split()]

        report = analyze_expenses(expenses)

        print("\nExpense Analysis")
        print("-" * 30)
        print(f"Days analyzed: {report['days_analyzed']}")
        print(f"Average per day: €{report['average_daily_expense']:.2f}")
        print(f"Highest expense: €{report['highest_expense']:.2f}")
        print(f"Total expenses: €{report['total_expenses']:.2f}")

        if report["alerts"]:
            print("\nUnusually high expenses:")

            for alert in report["alerts"]:
                print(
                    f"Day {alert['day']}: €{alert['amount']:.2f} "
                    f"(€{alert['difference']:.2f} above average)"
                )
        else:
            print("\nNo unusually high expenses detected.")

    except ValueError as error:
        print(f"Invalid input: {error}")
