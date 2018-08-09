# Upload-Image-File-to-Server-using-Almofire-

Reference Url : http://swiftdeveloperblog.com/image-upload-example/


// php code //

<?php
$firstName = $_POST["firstName"];
$lastName = $_POST["lastName"];
$userId = $_POST["userId"];

$target_dir = "uploads";


$target_dir = $target_dir . "/" . basename($_FILES["file"]["name"]);

if (move_uploaded_file($_FILES["file"]["tmp_name"], $target_dir)) 
{
echo json_encode([
"Message" => "The file ". basename( $_FILES["file"]["name"]). " has been uploaded.",
"Status" => "OK",
"userId" => $_REQUEST["userId"]
]);

} else {

echo json_encode([
"Message" => "Sorry, there was an error uploading your file.",
"Status" => "Error",
"userId" => $_REQUEST["userId"]
]);

}
?>

// Ios Code


 let param = [
            "firstName"  : "Sergey",
            "lastName"    : "Kargopolov",
            "userId"    : "9"
        ]
        // let imageToUploadURL = Bundle.main.url(forResource: "EmailIcon", withExtension: "png")
        // let image = UIImage.init(named: "myImage")
        
        let now = Date()
        
        let formatter = DateFormatter()
        
        formatter.timeZone = TimeZone.current
        
        formatter.dateFormat = "yyyyMMdd_HHmmss"
        
        let dateString = formatter.string(from: now)
        
        //Bank_Details_Proof_yyyyMMdd_HHmmss_9762204400.jpg
        let number = UserDefaults.standard.string(forKey: "mobilenom")!
        let imageName = "Bank_Details_Proof_ \(dateString)_\(number)"
        
        print("imageName",imageName)
        let imgData = UIImageJPEGRepresentation(image, 0.2)!
        
        let url = "https://www.stockaxis.com/UploadIOS.php"
        
        // Use Alamofire to upload the image
        Alamofire.upload(multipartFormData: { multipartFormData in
            multipartFormData.append(imgData, withName: "file",fileName: "\(imageName).jpg", mimeType: "image/jpg")
            for (key, value) in param {
                multipartFormData.append(value.data(using: String.Encoding.utf8)!, withName: key)
            } //Optional for extra parameters
        },
                         to: url,
                         encodingCompletion: { encodingResult in
                            switch encodingResult {
                            case .success(let upload, _, _):
                                upload.responseJSON { response in
                                    print(response.request!)  // original URL request
                                    print(response.response!) // URL response
                                    print(response.data!)     // server data
                                    print(response.result)   // result of response serialization
                                    
                                    if let JSON = response.result.value {
                                        print("JSON: \(JSON)")
                                    }
                                    if let jsonResponse = response.result.value as? [String: Any] {
                                        print("userid",jsonResponse["userId"]!)
                                    }
                                }
                            case .failure(let encodingError):
                                print(encodingError)
                            }
        }
        )
        
        
