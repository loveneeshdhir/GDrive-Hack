# Downloading view only protected PDF's from Google Drive

### Step 1 - Open the document in Google Docs
### Step 2 - Scroll to the bottom of the document, so all the pages are present
### Step 3 - Open Developer Tools on separate window and choose the Console tab
### Step 4 - Paste the code below (and hit enter)
```
let jspdf = document.createElement("script");
 
jspdf.onload = function () {
 
    let pdf = new jsPDF();
    let elements = document.getElementsByTagName("img");
    for (let i in elements) {
        let img = elements[i];
        console.log("add img ", img);
        if (!/^blob:/.test(img.src)) {
            console.log("invalid src");
            continue;
        }
        let can = document.createElement('canvas');
        let con = can.getContext("2d");
        can.width = img.width;
        can.height = img.height;
        con.drawImage(img, 0, 0, img.width, img.height);
        let imgData = can.toDataURL("image/jpeg", 1.0);
        pdf.addImage(imgData, 'JPEG', 0, 0);
        pdf.addPage();
    }
 
    pdf.save("download.pdf");
};
 
jspdf.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.debug.js';
document.body.appendChild(jspdf);
```

### Now the PDF should be downloaded

## What the script does?

It iterates trough the document checking for images (Google Drive stores pages as images) then writes it’s contents to a PDF.

## Note

1. It was tested on Chrome.
2. It converts pages to jpg images. Hence no gurantee of the text being preserved.
3. If you’re getting only some part of the document visible, try zooming out your browser, navigating the whole document to load the pages and then re-run the script.
