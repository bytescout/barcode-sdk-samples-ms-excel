## How to batch generate barcodes in cells with VBA in Microsoft Excel using ByteScout Barcode SDK

### How to code in Microsoft Excel to batch generate barcodes in cells with VBA with this step-by-step tutorial

The code below will help you to implement an Microsoft Excel app to batch generate barcodes in cells with VBA. ByteScout Barcode SDK: the robost library (Software Development Kit) that is designed for automatic generation of high-quality barcodes for printing, electronic documents and pdf. All popular barcode types are supported from Code 39 and Code 129 to QR Code, UPC, GS1, GS-128, Datamatrix, PDF417, Maxicode and many others. Provides support for full customization of fonts, colors, output and printing sizes. Special tools are included to verify output quality and printing quality. Can add generated barcode into new or existing documents, images and PDF. It can batch generate barcodes in cells with VBA in Microsoft Excel.

This rich sample source code in Microsoft Excel for ByteScout Barcode SDK includes the number of functions and options you should do calling the API to batch generate barcodes in cells with VBA. Just copy and paste the code into your Microsoft Excel application’s code and follow the instruction. Test Microsoft Excel sample code examples whether they respond your needs and requirements for the project.

ByteScout free trial version is available for download from our website. It includes all these programming tutorials along with source code samples.

## REQUEST FREE TECH SUPPORT

[Click here to get in touch](https://bytescout.zendesk.com/hc/en-us/requests/new?subject=ByteScout%20Barcode%20SDK%20Question)

or just send email to [support@bytescout.com](mailto:support@bytescout.com?subject=ByteScout%20Barcode%20SDK%20Question) 

## ON-PREMISE OFFLINE SDK 

[Get Your 60 Day Free Trial](https://bytescout.com/download/web-installer?utm_source=github-readme)
[Explore SDK Docs](https://bytescout.com/documentation/index.html?utm_source=github-readme)
[Sign Up For Online Training](https://academy.bytescout.com/)


## ON-DEMAND REST WEB API

[Get your API key](https://pdf.co/documentation/api?utm_source=github-readme)
[Explore Web API Documentation](https://pdf.co/documentation/api?utm_source=github-readme)
[Explore Web API Samples](https://github.com/bytescout/ByteScout-SDK-SourceCode/tree/master/PDF.co%20Web%20API)

## VIDEO REVIEW

[https://www.youtube.com/watch?v=REnj3A-oSPI](https://www.youtube.com/watch?v=REnj3A-oSPI)




<!-- code block begin -->

##### ****BarcodeGenerationCode_VB.txt:**
    
```
' IMPORTANT: This demo uses VBA so if you have it disabled please temporary enable
' by going to Tools - Macro - Security.. and changing the security mode to ""Medium""
' to Ask if you want enable macro or not. Then close and reopen this Excel document

' You should have evaluation version of the ByteScout SDK installed to get it working - get it from https://bytescout.com

' If you are getting error message like
' "File or assembly named Bytescout SDK, or one of its dependencies, was not found"
' then please try the following:
'
' - Close Excel
' - (for Office 2003 only) download and install this hotfix from Microsoft:
' http://www.microsoft.com/downloads/details.aspx?FamilyId=1B0BFB35-C252-43CC-8A2A-6A64D6AC4670&displaylang=en
'
' and then try again!
'
' If you have any questions please contact us at http://bytescout.com/support/ or at support@bytescout.com
                            

'==============================================
'References used
'=================
'Bytescout Barcode SDK
'
' IMPORTANT:
' ==============================================================
'1) Add the ActiveX reference in Tools -> References
'2) Loop through the values from the Column A for which barcode has to be generated
'3) Parse the value to Bytescout Barcode Object to generate the barcode using QR Code barcode type.
'4) Save the generated Barcode Image
'5) Insert the Barcode Image in the Column B
'6) Repeat the steps 3 to 5 till the last Value in Column A
'
'==================================================================

Option Explicit

' declare function to get temporary folder (where we could save barcode images temporary)
Declare Function GetTempPath _
Lib "kernel32" Alias "GetTempPathA" _
(ByVal nBufferLength As Long, _
ByVal lpBuffer As String) As Long
 
' function to return path to temporary folder
Public Function fncGetTempPath() As String
    Dim PathLen As Long
    Dim WinTempDir As String
    Dim BufferLength As Long
    BufferLength = 260
    WinTempDir = Space(BufferLength)
    PathLen = GetTempPath(BufferLength, WinTempDir)
    If Not PathLen = 0 Then
        fncGetTempPath = Left(WinTempDir, PathLen)
    Else
        fncGetTempPath = CurDir()
    End If
End Function


Sub Barcode_Click()

'Fetch the Worksheet
Dim mySheet As Worksheet
Set mySheet = Worksheets(1)                 'Barcode_Data Sheet

'temp path to save the Barcode images
Dim filePath As String
filePath = fncGetTempPath()            'Change the Path But should end with Backslash( \ )

'Prepare the Bytescout Barcode Object
'====================================
Dim myBarcode As New Bytescout_BarCode.Barcode

myBarcode.RegistrationName = "demo"         'Change the name for full version
myBarcode.RegistrationKey = "demo"          'Change the key for full version

'Barcode Settings
myBarcode.Symbology = SymbologyType_QRCode  ' QR Code barcode, you may change to other barcode types like Code 39, Code 128 etc

' set barcode image quality resolution
myBarcode.ResolutionX = 300                 'Resolution higher than 250 is good for printing
myBarcode.ResolutionY = 300                 'Resolution higher than 250 is good for printing

myBarcode.DrawCaption = True                'Showing Barcode Captions in the Barcode Image
myBarcode.DrawCaptionFor2DBarcodes = True   ' show captions for 2D barcodes like QR Code

' first clean the B column from old images (if any)
Dim Sh As Shape
With mySheet
   For Each Sh In .Shapes
       If Not Application.Intersect(Sh.TopLeftCell, .Range("B1:B50")) Is Nothing Then
         If Sh.Type = msoPicture Then Sh.Delete
       End If
    Next Sh
End With

' now generate new barcodes and insert into cells in the column B
' Repeat the steps for each row from 2 to 6
Dim myVal As Integer

For myVal = 2 To 6                          'change the code to all rows with values
    'Parse the Value from the Column A to Bytescout Barcode Object
    myBarcode.Value = mySheet.Cells(myVal, 1).Text
    'Fit the barcode into 80X30 mm rectangle
    myBarcode.FitInto_3 80, 30, 4           '4 refers to units of measurement as millimeter
    'Save the barcode image to a file in temporary folder
    myBarcode.SaveImage filePath & "myBarcode" & myVal & ".png"
    
    'Insert the Barcode image to the Column B and resize them to fit the cell.
    '==========================================================================
    With mySheet.Pictures.Insert(filePath & "myBarcode" & myVal & ".png")
        .ShapeRange.LockAspectRatio = True ' lock aspect ratio
        .Left = mySheet.Cells(myVal, 2).Left + 1 ' set left
        .Top = mySheet.Cells(myVal, 2).Top + 1 ' set right
        .PrintObject = True ' allow printing this object
        .Placement = xlMove ' set placement mode to move but do not resize with the cell
        .ShapeRange.ScaleHeight 1, True ' set height scale to 1 (no scale)
        .ShapeRange.ScaleWidth 1, True ' set width scale to 1 (no scale)
    End With
    
    
Next myVal ' move to next cell in the column

' Release the Barcode Object.
Set myBarcode = Nothing

End Sub

```

<!-- code block end -->    

<!-- code block begin -->

##### ****Reference Error - README.txt:**
    
```
' IMPORTANT: This demo uses VBA so if you have it disabled please temporary enable
' by going to Tools - Macro - Security.. and changing the security mode to ""Medium""
' to Ask if you want enable macro or not. Then close and reopen this Excel document

' You should have evaluation version of the ByteScout SDK installed to get it working - get it from https://bytescout.com

' If you are getting error message like
' "File or assembly named Bytescout SDK, or one of its dependencies, was not found"
' then please try the following:
'
' - Close Excel
' - (for Office 2003 only) download and install this hotfix from Microsoft:
' http://www.microsoft.com/downloads/details.aspx?FamilyId=1B0BFB35-C252-43CC-8A2A-6A64D6AC4670&displaylang=en
'
' and then try again!
'
' If you have any questions please contact us at http://bytescout.com/support/ or at support@bytescout.com
                            



```

<!-- code block end -->