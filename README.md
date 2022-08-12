# BIM-Codes-Portal
BIM Dynamo Scripts


HOW TO CREATE LEGENDS ON MULTIPLE SHEETS

public void legendOnSheets()
{
    Document doc = this.ActiveUIDocument.Document;
    
    // create list of element ids for the sheets that will get the legends
    List<ElementId> sheetIds = new List<ElementId>();
        
    // use SheetNumber to find the desired sheets
    // add each sheet to the list of sheet ids
    sheetIds.Add(new FilteredElementCollector(doc)
        .OfClass(typeof(ViewSheet))
        .Cast<ViewSheet>()
        .FirstOrDefault(q => q.SheetNumber == "A101").Id);
    sheetIds.Add(new FilteredElementCollector(doc)
        .OfClass(typeof(ViewSheet))
        .Cast<ViewSheet>()
        .FirstOrDefault(q => q.SheetNumber == "A102").Id);                
    sheetIds.Add(new FilteredElementCollector(doc)
        .OfClass(typeof(ViewSheet))
        .Cast<ViewSheet>()
        .FirstOrDefault(q => q.SheetNumber == "A103").Id);    
    
    // find the legend to put on the sheets
    // use ViewType.Legend and the view name to find the legend view
    Element legend = new FilteredElementCollector(doc)
        .OfClass(typeof(View))
        .Cast<View>()
        .FirstOrDefault(q => q.ViewType == ViewType.Legend && q.Name == "Legend 1");
    
    // create a transaction so that the document can be modified
    using (Transaction t = new Transaction(doc, "Legends on Sheets"))
    {
        // start the transaction
        t.Start();
        
        // loop through the list of sheet ids
        foreach (ElementId sheetid in sheetIds)
        {
            // create a new viewport for the legend on each sheet
            // place the legend view at the 0,0,0 location on each sheet
            // user will need to move the legend to the desired location
            Viewport.Create(doc, sheetid, legend.Id, new XYZ(0,0,0));
        }
        
        // commit the changes
        t.Commit();
    }
}
