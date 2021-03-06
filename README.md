# ASP.NET-MVC-jQuery-Datatables
## Price Quotation Table with jQuery Datatables (ASP.NET MVC)

In order to make datatable work, we should place our scripts in the following order.

```C#
<link href="https://cdn.datatables.net/v/dt/dt-1.10.18/datatables.min.css" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.10.19/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/dataTables.buttons.min.js" ></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/buttons.flash.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.1.3/jszip.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.36/pdfmake.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.36/vfs_fonts.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/buttons.html5.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/buttons.print.min.js"></script>
```

### T-SQL Database Code:

```SQL
CREATE TABLE [dbo].[Table] (
    [QuoteID]      NCHAR (10) NOT NULL,
    [CustomerName] NCHAR (40) NOT NULL,
    [Description]  NCHAR (40) NOT NULL,
    [Quantity]     FLOAT (53) NOT NULL,
    [UnitPrice]    FLOAT (53) NOT NULL,
    [VatRate]      FLOAT (53) NOT NULL,
    [SubTotal]     FLOAT (53) NOT NULL,
    [TotalAmount]  FLOAT (53) NOT NULL,
    PRIMARY KEY CLUSTERED ([QuoteID] ASC)
);
```
### HomeController.cs;
```ASP.NET
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;

namespace MVCDatatable.Controllers
{
    public class HomeController : Controller
    {
       public ActionResult Index()
        {
            return View();
        }

        public ActionResult loaddata()
        {
            using (MyDatabaseEntities dc = new MyDatabaseEntities())
            {
                var data = dc.Tables.OrderBy(a => a.QuoteID).ToList();
                return Json(new { data = data }, JsonRequestBehavior.AllowGet);
            }
        }
    }
}
```

### View for Index;

```ASP.NET

@{
    ViewBag.Title = "Index";
}

<h2>Price Quotation List</h2><br />
<div style="margin:0 auto;">
    <table style="width:100%;" class="display" id="QuoteTable">
        <thead>
            <tr>
                <th>Quote ID</th>
                <th>Customer</th>
                <th>Description</th>
                <th>Quantity</th>
                <th>Unit Price</th>
                <th>VAT/Tax Rate</th>
                <th>Net Offer</th>
                <th>Total</th>
            </tr>
        </thead>
    </table>
</div>


@*Datatable CSS*@
<link href="https://cdn.datatables.net/v/dt/dt-1.10.18/datatables.min.css" rel="stylesheet" />

@*Datatable-buttons CSS*@
<link href="https://cdn.datatables.net/buttons/1.5.4/css/buttons.dataTables.min.css" rel="stylesheet" />

@*jQuery JS*@
@Scripts.Render("~/bundles/jquery")
<script src="~/Scripts/jquery-3.3.1.min.js"></script>

@*Datatable JS*@
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.10.19/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/dataTables.buttons.min.js" ></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/buttons.flash.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.1.3/jszip.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.36/pdfmake.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.36/vfs_fonts.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/buttons.html5.min.js"></script>
<script type="text/javascript" language="javascript" src="https://cdn.datatables.net/buttons/1.5.4/js/buttons.print.min.js"></script>

<script>
    $(document).ready(function () {
        $.noConflict();
        $('#QuoteTable').DataTable({

            dom: 'Bfrtip',
            buttons: [
                {
                    extend: 'copy',
                    title: 'Quote Report'
                },
                {
                    extend: 'csv',
                    title: 'Quote Report'
                },
                 {
                     extend: 'excel',
                     title: 'Quote Report'
                },
                  {
                    extend: 'pdf',
                      title: 'Quote Report'
                },
                   {
                       extend: 'print',
                       title: 'Quote Report'
                }
            ],

            "ajax": {
                "url": "/home/loaddata",
                "type": "GET",
                "datatype": "json",
            },
            "language": {
                "zeroRecords": "No records found to display"
            },
           
            "columns": [
                { "data": "QuoteID", "autoWidth": true },
                { "data": "CustomerName", "autoWidth": true },
                { "data": "Description", "autoWidth": true },
                { "data": "Quantity", "autoWidth": true },
                { "data": "UnitPrice", "autoWidth": true },
                { "data": "VatRate", "autoWidth": true },
                { "data": "SubTotal", "autoWidth": true },
                { "data": "TotalAmount", "autoWidth": true }
            ]
        });
    });
</script>

```

![alt text](https://i.ibb.co/s68fn03/a1.png)


![alt text](https://i.ibb.co/C63D2z7/a2.png)


![alt text](https://i.ibb.co/ysXCYT3/Przechwytywanie.png)


![alt text](https://i.ibb.co/4dnyWHQ/Przechwytywanie.png)


