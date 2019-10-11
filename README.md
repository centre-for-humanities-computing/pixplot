# CHCAA Pix plot Vores Museum
A clone of DHLab's pix plot repository adapted for use ith vores museum's data

## PixPlot

This repository contains code that can be used to visualize tens of thousands of images in a two-dimensional projection within which similar images are clustered together. The image analysis uses Tensorflow's Inception bindings, and the visualization layer uses a custom WebGL viewer.

![App preview](./assets/images/preview.png?raw=true)

## Dependencies

You need to [install Docker](https://docs.docker.com/install/). If you are on Windows 7 or earlier, you may need to install [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/) instead.

The html viewer requires a WebGL-enabled browser.

## How To Generate the Pixplot

Download this repository by clicking the green "Clone or download" button and then "Download ZIP".

Unpack the zip file. 

Start a terminal, cd into the folder that contains this README file.

*Below steps each have numbered commands for later reference.*

Generate the environment for your pixplot within a docker container (command 1):

```bash
# command 1: 
# build the docker container
docker build --tag pixplot --file Dockerfile .
```

Process your collection into a pix plot (command 2). 

*Depending on the size of your image collection, this can take several hours. In our hackathon it took Max around 3.5 hours.*

```
# command 2:
# process images from the VM collection
# use the `-v` flag to mount directories from outside
#   the container into the container
docker run \
  -v "$(pwd)/output:/pixplot/output" \
  -v "$(pwd)/VoresMuseum/images:/pixplot/images" \
  pixplot \
  bash -c "cd pixplot && python3.6 utils/process_images.py images/*"
```

You now have generated your pixplot. The next step will start a web server to host your plot on http://localhost:5000

```
# command3:
# run the web server
docker run \
  -v "$(pwd)/output:/pixplot/output" \
  -p 5000:5000 \
  pixplot \
  bash -c "cd pixplot && python3.6 -m http.server 5000"
```

## Curating Automatic Hotspots

By default, PixPlot uses [*k*-means clustering](https://en.wikipedia.org/wiki/K-means_clustering) to find twenty hotspots in the visualization.  You can adjust the number of discovered hotspots by changing the `n_clusters` value in `utils/process_images.py` and re-running the script.

After processing, you can curate the discovered hotspots by editing the resulting `output/plot_data.json` file. (This file can be unwieldy in large datasets -- you may wish to disable syntax highlighting and automatic wordwrap in your text editor.) The hotspots will be listed at the very end of the JSON data, each containing a label (by default 'Cluster *N*') and the name of an image that represents the centroid of the discovered hotspot.

You can add, remove or re-order these, change the labels to make them more meaningful, and/or adjust the image that symbolizes each hotspot in the left-hand **Hotspots** menu.  *Hint: to get the name of an image that you feel better reflects the cluster, click on it in the visualization and it will appear suffixed to the URL.*


## Acknowledgements

The DHLab would like to thank [Cyril Diagne](http://cyrildiagne.com/), a lead developer on the spectacular [Google Arts Experiments TSNE viewer](https://artsexperiments.withgoogle.com/tsnemap/), for generously sharing ideas on optimization techniques used in this viewer.

## Notes
### pre-processing
Build the docker container. (first docker command above)

Attempt to process images by running the second docker command (may take a few minutes if uyou have broken files). This will produce a list of broken files. 

Then remove the files as per requested by the pixplot output

In our case we ran: 
```$xslt
 cd VoresMuseum
 rm images/image10601.jpg images/image10834.jpg images/image11800.jpg images/image1554.jpg images/image18647.jpg images/image19617.jpg images/image19622.jpg images/image19768.jpg images/image19972.jpg images/image21209.jpg images/image22110.jpg images/image23534.jpg images/image24355.jpg images/image25214.jpg images/image25427.jpg images/image25642.jpg images/image26440.jpg images/image26804.jpg images/image2687.jpg images/image28244.jpg images/image3005.jpg images/image31008.jpg images/image31675.jpg images/image32269.jpg images/image32951.jpg images/image3435.jpg images/image34807.jpg images/image36176.jpg images/image37445.jpg images/image41207.jpg images/image43329.jpg images/image44643.jpg images/image48661.jpg images/image48964.jpg images/image50512.jpg images/image51428.jpg images/image54582.jpg images/image54769.jpg images/image56676.jpg images/image57915.jpg images/image5807.jpg images/image62564.jpg images/image63390.jpg images/image64199.jpg images/image64329.jpg images/image65129.jpg images/image66153.jpg images/image67607.jpg images/image68549.jpg images/image69343.jpg images/image69469.jpg images/image69698.jpg images/image71051.jpg images/image71622.jpg images/image72178.jpg images/image75209.jpg images/image75438.jpg images/image75549.jpg images/image76409.jpg images/image76935.jpg images/image77429.jpg images/image77755.jpg images/image8495.jpg images/image8534.jpg images/image9337.jpg images/image9770.jpg images/image9789.jpg
 cd ..
```

Run the second docker command again (again: may take a few minutes)

we found images with text describing missing images which were removed with

```$xslt
cd SMK/images
rm image24817.jpg image52451.jpg image64468.jpg image76990.jpg image38060.jpg image20536.jpg image40878.jpg image50382.jpg image268.jpg image9454.jpg image56285.jpg image18776.jpg image44206.jpg image68829.jpg image36244.jpg image46492.jpg image13543.jpg image9972.jpg image2671.jpg image46118.jpg image4255.jpg image23185.jpg image62522.jpg image70460.jpg image20189.jpg image48927.jpg image27166.jpg image80630.jpg image77681.jpg image79110.jpg image17413.jpg image4909.jpg image12518.jpg image71588.jpg image68100.jpg image20753.jpg image34716.jpg image13507.jpg image66263.jpg image74574.jpg image60449.jpg image60364.jpg image42985.jpg image51722.jpg image1210.jpg image57635.jpg image43708.jpg image48606.jpg image79951.jpg image5734.jpg image77302.jpg image78058.jpg image4878.jpg image8010.jpg image30679.jpg image57390.jpg image55832.jpg image41867.jpg image667.jpg image40218.jpg image61623.jpg image44178.jpg image66741.jpg image56093.jpg image59218.jpg image23315.jpg image16352.jpg image15236.jpg image58608.jpg image66733.jpg image72776.jpg image22081.jpg image39327.jpg image58376.jpg image50822.jpg image2381.jpg image50431.jpg image31494.jpg image54030.jpg image48857.jpg image60679.jpg image57679.jpg image4357.jpg image5112.jpg image34947.jpg image47327.jpg image24078.jpg image70809.jpg image71694.jpg image80151.jpg image38839.jpg image71806.jpg image76928.jpg image62760.jpg image46126.jpg image5134.jpg image67703.jpg image47301.jpg image15963.jpg image74329.jpg image29353.jpg image10503.jpg image78434.jpg image37221.jpg image76662.jpg image46880.jpg image45628.jpg image52507.jpg image53845.jpg image10115.jpg image78667.jpg image67785.jpg image37376.jpg image54861.jpg image47707.jpg image80496.jpg image55645.jpg image43635.jpg image48336.jpg image74916.jpg image57063.jpg image64462.jpg image40461.jpg image15397.jpg image78486.jpg image70987.jpg image80970.jpg image47378.jpg image68499.jpg image34463.jpg image69585.jpg image5100.jpg image13498.jpg image70335.jpg image56487.jpg image29481.jpg image357.jpg image79072.jpg image49507.jpg image54501.jpg image77269.jpg image66572.jpg image61017.jpg image40067.jpg image3043.jpg image15614.jpg image60582.jpg image12371.jpg image70955.jpg image31356.jpg image24134.jpg image19520.jpg image244.jpg image43480.jpg image28502.jpg image64263.jpg image67061.jpg image54470.jpg image39652.jpg image52322.jpg image8820.jpg image13491.jpg image3455.jpg image42472.jpg image39487.jpg image59605.jpg image67599.jpg image15212.jpg image10033.jpg image21242.jpg image49646.jpg image27345.jpg image39141.jpg image57452.jpg image74761.jpg image38178.jpg image38989.jpg image80872.jpg image46135.jpg image76528.jpg image64159.jpg image43299.jpg image37763.jpg image9434.jpg image32223.jpg image80707.jpg image10604.jpg image8982.jpg image49467.jpg image80728.jpg image10743.jpg image38243.jpg image28607.jpg image55880.jpg image48947.jpg image60883.jpg image41002.jpg image80903.jpg image57314.jpg image53028.jpg image50517.jpg image1788.jpg image45189.jpg image36753.jpg image4597.jpg image31385.jpg image5517.jpg image69192.jpg image66309.jpg image53401.jpg image43582.jpg image61631.jpg image4128.jpg image18165.jpg image6303.jpg image39436.jpg image54327.jpg image18309.jpg image54500.jpg image52417.jpg image26144.jpg image9235.jpg image13260.jpg image26578.jpg image72429.jpg image47249.jpg image79790.jpg image76473.jpg image52257.jpg image50437.jpg image36281.jpg image51055.jpg image11090.jpg image43319.jpg image44909.jpg image64492.jpg image41812.jpg image30774.jpg image24731.jpg image5325.jpg image10382.jpg image45253.jpg image55052.jpg image2640.jpg image18351.jpg image11496.jpg image65004.jpg image42440.jpg image72253.jpg image59893.jpg image37409.jpg image43912.jpg image79235.jpg image80663.jpg image40876.jpg image42949.jpg image36841.jpg image63161.jpg image76556.jpg image15393.jpg image74310.jpg image58857.jpg image9280.jpg image13781.jpg image66242.jpg image70232.jpg image68402.jpg image38862.jpg image8445.jpg image45851.jpg image46485.jpg image36125.jpg image30479.jpg image42223.jpg image15938.jpg image39109.jpg image50534.jpg image62547.jpg image63920.jpg image40965.jpg image34660.jpg image63366.jpg image76741.jpg image62227.jpg image1062.jpg image57318.jpg image54637.jpg image25839.jpg image5767.jpg image77504.jpg image15917.jpg image52648.jpg image77290.jpg image26436.jpg image80464.jpg image17126.jpg image71780.jpg image39786.jpg image40756.jpg image65023.jpg image28565.jpg image78353.jpg image5496.jpg image55689.jpg image17144.jpg image57648.jpg image30836.jpg image11723.jpg image48156.jpg image44856.jpg image31612.jpg image7692.jpg image18288.jpg image75599.jpg image23444.jpg image36491.jpg image6321.jpg image63428.jpg image52721.jpg image75520.jpg image29138.jpg image15784.jpg image58487.jpg image39888.jpg image38181.jpg image62824.jpg image9608.jpg image37455.jpg image34647.jpg image12024.jpg image24730.jpg image64951.jpg image19334.jpg image44715.jpg image5996.jpg image46730.jpg image69035.jpg image31679.jpg image67089.jpg image67297.jpg image76535.jpg image21903.jpg image71326.jpg image60887.jpg image74051.jpg image34875.jpg image51430.jpg image16180.jpg image73534.jpg image71501.jpg image69564.jpg image69612.jpg image7935.jpg image51499.jpg image25736.jpg image58226.jpg image15591.jpg image64991.jpg image33668.jpg image45820.jpg image17220.jpg image26023.jpg image53832.jpg image33329.jpg image48365.jpg image47298.jpg image25877.jpg image80222.jpg image28697.jpg image48136.jpg image23213.jpg image19924.jpg image64229.jpg image24813.jpg image71255.jpg image56054.jpg image73308.jpg image55471.jpg image21569.jpg image76683.jpg image80126.jpg image13039.jpg image13029.jpg image35141.jpg image10365.jpg image66197.jpg image22553.jpg image15070.jpg image13201.jpg image50683.jpg image18432.jpg image3883.jpg image43455.jpg image27839.jpg image13263.jpg image43846.jpg image67563.jpg image16182.jpg image52620.jpg image12939.jpg image50050.jpg image59933.jpg image19742.jpg image58224.jpg image52945.jpg image9647.jpg image11005.jpg image63830.jpg image27405.jpg image10136.jpg image38681.jpg image8257.jpg image10816.jpg image63276.jpg image77700.jpg image1737.jpg image61522.jpg image52766.jpg image63517.jpg image2254.jpg image74273.jpg image18889.jpg image12351.jpg image18694.jpg image77839.jpg image48163.jpg image34179.jpg image38088.jpg image27118.jpg image32731.jpg image54197.jpg image43444.jpg image42673.jpg image55434.jpg image19345.jpg image69851.jpg image55107.jpg image37501.jpg image56299.jpg image30302.jpg image498.jpg image65074.jpg image2007.jpg image9941.jpg image39778.jpg image26897.jpg image9542.jpg image53086.jpg image1843.jpg image29802.jpg image34757.jpg image13075.jpg image76294.jpg image56102.jpg image48482.jpg image20.jpg image19224.jpg image61559.jpg image18119.jpg image79691.jpg image53823.jpg image43954.jpg image2987.jpg image45248.jpg image30847.jpg image69298.jpg image64906.jpg image13122.jpg image27575.jpg image54828.jpg image30379.jpg image36002.jpg image305.jpg image80329.jpg image49790.jpg image56672.jpg image76106.jpg image57997.jpg image34917.jpg image50499.jpg image14600.jpg image56136.jpg image33124.jpg image54252.jpg
cd ../..
```

### second rendering
the second rendering of the cleaned pix plot had some extreme outliers (x ~= -20000). another rendering could integrate all images better. Such outlying clusters can be caused by a group of similar motives emphasizing their distance to other clusters.
