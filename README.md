# fpdi-php-password-protection
fpdi password protection


    <?php
        function pdfEncrypt ($origFile, $password, $destFile){
            require_once('fpdi/FPDI_Protection.php');
            $pdf = new FPDI_Protection();
            
            // set the format of the destinaton file, in our case 6×9 inch
            //$pdf->FPDF('P', 'in', array('6','9'));
            $pdf->FPDF('P', 'in');
            
            //Calculate the number of pages from the original document.
            $pagecount = $pdf->setSourceFile($origFile);
            //Copy all pages from the old unprotected pdf in the new one.
            for ($loop = 1; $loop <= $pagecount; $loop++) {
                $tplidx = $pdf->importPage($loop);
                $pdf->addPage();
                $pdf->useTemplate($tplidx);
            }
    
            //Protect the new pdf file, and allow no printing, copy, etc. and
            //leave only reading allowed.
            $pdf->SetProtection(array(), $password);
            $pdf->Output($destFile, 'F');
            return $destFile;
        }
    
        //Password for the PDF file (I suggest using the email adress of the purchaser).
        $password = "password";
        //Name of the original file (unprotected).
        $origFile = "pdf.pdf";
        //Name of the destination file (password protected and printing rights removed).
        $destFile ="sample.pdf";
        //Encrypt the book and create the protected file.
        pdfEncrypt($origFile, $password, $destFile );
    ?>


