# Mask Detector With Pytorch YOLO

## Dataset

+ Face-mask dataset

  + 7958 items

  + Database structure:

    ```
    data
    ├── Annotations
    │   ├── id.xml
    ...
    ├── JPEGImages
    │   ├── id.jpg
    ```

  + Annotation: PASCAL VOC2007:

    ```xml
    <annotation>
        <folder>VOC2007</folder>
        <filename>1_Handshaking_Handshaking_1_35.jpg</filename>
        <source>
            <database>The VOC2007 Database</database>
            <annotation>PASCAL VOC2007 </annotation>
            <image>flickr</image>
        </source>
        <size>
            <width>1024</width>
            <height>1392</height>
            <depth>3</depth>
        </size>
        <segmented>0</segmented>
        <object>
        	<name>face</name>
        	<pose>Unspecified</pose>
        	<truncated>0</truncated>
        	<difficult>0</difficult>
            <bndbox>
            <xmin>440</xmin>
            <ymin>141</ymin>
            <xmax>633</xmax>
            <ymax>399</ymax>
            </bndbox>
        </object>
    </annotation>
    ```

    + name: face (no mask), face_mask(with mask)