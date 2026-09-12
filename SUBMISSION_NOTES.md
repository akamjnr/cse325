# CSE325 Assignment Submission Notes

## Part 1: ASP.NET Core Web API

I completed the ASP.NET Core Web API CRUD operations in the PizzaStore project.

The original pizza records were:

- ID 1 – Classic Italian – Gluten Free: No
- ID 2 – Veggie – Gluten Free: Yes

I added the following additional pizza record:

- ID 4 – Hawaiian – Gluten Free: No

The final GET request confirmed the existing pizzas and the additional Hawaiian pizza:

```text
HTTP/1.1 200 OK
```

```json
[
  {
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
  },
  {
    "id": 2,
    "name": "Veggie",
    "isGlutenFree": true
  },
  {
    "id": 4,
    "name": "Hawaiian",
    "isGlutenFree": false
  }
]
```

## Part 2: Sales Summary Report

I added a sales summary function to the .NET file and directory application. The function generates a report containing the total sales and the sales total for each sales file.

The generated report produced a total sales amount of:

```text
$2,012.20
```

### Sales Summary Function

```csharp
void GenerateSalesSummaryReport(
    IEnumerable<string> salesFiles,
    double salesTotal,
    string salesTotalDir)
{
    StringBuilder report = new StringBuilder();

    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");
    report.AppendLine($"Total Sales: {salesTotal.ToString("C")}");
    report.AppendLine();
    report.AppendLine("Details:");

    foreach (var file in salesFiles)
    {
        string salesJson = File.ReadAllText(file);

        SalesData? data =
            JsonConvert.DeserializeObject<SalesData?>(salesJson);

        double fileTotal = data?.Total ?? 0;

        report.AppendLine(
            $"  {Path.GetFileName(file)}: {fileTotal.ToString("C")}"
        );
    }

    File.WriteAllText(
        Path.Combine(salesTotalDir, "salesSummary.txt"),
        report.ToString()
    );
}
```