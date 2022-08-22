# Implementation Details

## Major Tools Used

- Trivy
- jq
- sed

**Trivy** was used to do the image scanning. This was also used with the `--template` flag to make it easy to get a simpler output which can be used to check if an image is `SAFE` or `UNSAFE`.

**jq** was used to created the JSON array containing the image name and its status

**sed** was used to do some formating and stripping of the initial input, which is a list of images to be scanned.

## How Does it work?

- When an issue is created with the body containing a list of images to scan, the pipline is triggered. The  `init` step installs all required packages includig `trivy` the vulnerability scanning tool. 

- The next step is where all the main work is. Firstly a variable called `PASS` was created which contains some data which would be used for comparison later in the script. 

- Next, a cleanup of the images to be scanned is done, stripping of the brackets, commas, e.t.c in order to loop through the image list easily.

- Next an array was declared based on the cleaned image list created previously.

- From the array declared above, a for loop is done to loop through all the images in the array

- Then for each image we run the trivy scan command to do the vulnerability scan on the images. I also made sure to specify the output template which i'll talk about later in this file.

- The output of the trivy scan is stored in a file, then the last line of the output is captured which is compared to the `PASS` variable in step 2.

- The output contains each Vulnerabity level/type and the no of times it occurs. And the `PASS` variable contains the output for an image scan output that's `SAFE` without any vulnerabilities.

- So if there is a match between the image scan output and the `PASS` value, it means the image is `SAFE`, else its deemed `UNSAFE`.

- The scan status of each image is stored in a file called `result.txt`. This will be used in the next step.

- The next step named `FINAL` uses the file called `result.txt` created in the previous step to create a json array of the images and their status. Then the json array is assigned to a output variable called `FINAL_OUT` which will be used in the next step

- The next step using the `FINAL_OUT` output from the previous step as a message to reply to the issue created at the start.

## And thats how it works!!