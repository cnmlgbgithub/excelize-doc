# Трехмерная линейная диаграмма {#line3D}

Например, добавьте диаграмму, подобную этой:

<p align="center"><img width="770" src="../images/3d_line_chart.png" alt="создать Трехмерная линейная диаграмма с Excelize с помощью Go"></p>

```go
package main

import (
    "fmt"

    "github.com/xuri/excelize/v2"
)

func main() {
    f := excelize.NewFile()
    defer func() {
        if err := f.Close(); err != nil {
            fmt.Println(err)
        }
    }()
    if err := f.SetSheetName("Sheet1", "Лист1"); err != nil {
        fmt.Println(err)
        return
    }
    for idx, row := range [][]interface{}{
        {nil, "Apple", "Orange", "Pear"},
        {"Small", 2, 3, 3},
        {"Normal", 5, 2, 4},
        {"Large", 6, 7, 8},
    } {
        cell, err := excelize.CoordinatesToCellName(1, idx+1)
        if err != nil {
            fmt.Println(err)
            return
        }
        if err := f.SetSheetRow("Лист1", cell, &row); err != nil {
            fmt.Println(err)
            return
        }
    }
    if err := f.AddChart("Лист1", "E1", &excelize.Chart{
        Type: excelize.Line3D,
        Series: []excelize.ChartSeries{
            {
                Name:       "Лист1!$A$2",
                Categories: "Лист1!$B$1:$D$1",
                Values:     "Лист1!$B$2:$D$2",
            },
            {
                Name:       "Лист1!$A$3",
                Categories: "Лист1!$B$1:$D$1",
                Values:     "Лист1!$B$3:$D$3",
            },
            {
                Name:       "Лист1!$A$4",
                Categories: "Лист1!$B$1:$D$1",
                Values:     "Лист1!$B$4:$D$4",
            },
        },
        Format: excelize.GraphicOptions{
            OffsetX: 15,
            OffsetY: 10,
        },
        Legend: excelize.ChartLegend{
            Position: "top",
        },
        Title: excelize.ChartTitle{
            Name: "Трехмерная линейная диаграмма",
        },
        PlotArea: excelize.ChartPlotArea{
            ShowCatName:     false,
            ShowLeaderLines: false,
            ShowPercent:     false,
            ShowSerName:     false,
            ShowVal:         false,
        },
        ShowBlanksAs: "zero",
    }); err != nil {
        fmt.Println(err)
        return
    }
    // Сохранить workbook
    if err := f.SaveAs("Книга1.xlsx"); err != nil {
        fmt.Println(err)
    }
}
```
