public ActionResult Index(HttpPostedFileBase file)  
{  
  
    string ext = null;  
  
    string[] file_hexa_signature = {  
        "38-42-50-53", "25-50-44-46", "4D-4D-00-2A", "4D-4D-00-2A"  
    };  
  
    try  
  
    {  
  
        ext = System.IO.Path.GetExtension(file.FileName).ToLower();  
  
    } catch (Exception ex)  
  
    {  
  
        ViewBag.error = ex.Message;  
  
    }  
  
    if (ext == null)  
  
    {  
  
        ViewBag.error = "please select onlt psd, pdf, tif file types only";  
  
    }  
  
  
    if (file != null && file.ContentLength > 0)  
  
    {  
  
        string str = Path.GetFileName(file.FileName);  
  
        string path = Path.Combine(Server.MapPath("~/Docs"),  
  
            Path.GetFileName(file.FileName));  
  
        file.SaveAs(path);  
  
        BinaryReader reader = new BinaryReader(new FileStream(Convert.ToString(path), FileMode.Open, FileAccess.Read, FileShare.None));  
  
        reader.BaseStream.Position = 0x0; // The offset you are reading the data from  
  
        byte[] data = reader.ReadBytes(0x10); // Read 16 bytes into an array  
  
        string data_as_hex = BitConverter.ToString(data);  
  
        reader.Close();  
  
        // substring to select first 11 characters from hexadecimal array  
  
        string my = data_as_hex.Substring(0, 11);  
  
        string output = null;  
  
        switch (my)  
  
        {  
  
            case "38-42-50-53":  
  
                output = " => psd";  
  
                break;  
  
            case "25-50-44-46":  
  
                output = " => pdf";  
  
                break;  
  
            case "49-49-2A-00":  
  
                output = " => tif";  
  
                break;  
  
            case "4D-4D-00-2A":  
  
                output = " => tif";  
  
                break;  
  
            case "null":  
  
                output = "file type is not matches with array";  
  
                break;  
  
        }  
  
        ViewBag.Message = data_as_hex;  
  
        ViewBag.signature = my;  
  
        ViewBag.out_put = output;  
  
    }  
  
    return View();  
  
}  
