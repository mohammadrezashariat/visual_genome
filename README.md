# Script for working with Visual Genome
---

## Overview

 * Development and execution environment:
   * ubuntu 14.04LTS(64 bit)
   * python 3.4.5


 * Visual Genome:
     * Visual Genome:(http://visualgenome.org/)
     * download page: [Visual Genome Dataset](http://visualgenome.org/api/v0/api_home.html)

     * Dataset to be used:
       * [Download images part1(9.2GB)](https://cs.stanford.edu/people/rak248/VG_100K_2/images.zip)
       * [Download images part2(5.47GB)](https://cs.stanford.edu/people/rak248/VG_100K_2/images2.zip)
       * [Download objects(413.87)](http://visualgenome.org/static/data/dataset/objects.json.zip)


 * script:
   * extract_contents.py
     * A script that sorts words (synsets) representing objects in an image in order of appearance frequency
     * Used to confirm the type of object included in the image.  

   * extract_image.py
     * A script that outputs a list of images containing an object from the words (synsets) representing the object in the image
     * Used for image extraction

   * extract_bbox_by_image_list.py
     * Script that extracts the bounding box (bbox) in which the object is located from the words (synsets) that represent the object in        the image
     * Used for object position extraction (x, y, width, height)

   * extract_bbox_to_normalize.py
     * A script that extracts the range (Bounding Box; bbox) in which the object is located from the words (synsets) representing the          object in the image by normalizing (represented by 0 to 1) the vertical and horizontal sizes of the image
     * Used for object position extraction (x, y, width, height)


## Execution procedure

 1. Download datasets and scripts
   * Download from the data set link above.
   * **n the following, we assume the following directory structure.**
   ```
     visual_genome (folder)
       |- images (Folder; integrated downloaded part1 and part2. Create your own)
       |- scripts (Folder; store this script group)
       |- objects.json (File)
   ```

 2. Move to scripts folder
   ```
     cd scripts
   ```

 3. cript execution
   * Run extract_contents.py to check the type of synsets
     ```
       python extract_contents.py
     ```

     * Check that ./results/contents.csv is generated as the execution result.
       ```
         less ./results/contents.csv
       ```
     * synsets is a string described in the first column (name column) of the file.

   * If you find your favorite synsets, to extract the image that contains it, 69  Execute extract_image.py

     * When extracting "100" sheets by specifying "clock.n.01" as synsets,
       ```
         python extract_image.py -t clock.n.01 -n 100
       ```

       * heck that ./results/image_list.txt is generated as the execution result.
       ```
         less ./results/image_list.txt
       ```

   * If you want to extract location information (Bounding Box) from synsets and images that contain 81  Run                e                xtract_bbox_by_image_list.py

     * When "clock.n.01" is specified as synsets and "./results/image_list.txt" is specified as an image list, 83,
       ```
         python extract_bbox_by_image_list.py -s clock.n.01 -i ./results/image_list.txt
       ```

       * Check that the file is generated under ./out/ as the execution result.
       ```
         ls ./out/
       ```

   * If the normalized version of the Bounding Box is good, 93  Run extract_bbox_to_normalize.py
     * When "clock.n.01" is specified as synsets and "./results/image_list.txt" is specified as an image list,
       ```
         python extract_bbox_to_normalize.py -s clock.n.01 -i ./results/image_list.txt
       ```

　* Refer to the comments at the top of each script for details, as options may be specified in addition to the above method.


## Recommended usage method
 * **The following assumes commands from the parent directory of scripts (visual_genome).**

 * Gathers specific images from the image list (./scripts/results/image_list.txt) of the execution results of extract_image.py to a        directory.
   * Create an image_part folder and collect it in it
   ```
     mkdir image_part
     cat ./scripts/results/image_list.txt | xargs -I{} cp ./images/{} ./image_part/
   ```

---
