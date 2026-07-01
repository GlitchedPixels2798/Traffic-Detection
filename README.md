# Traffic-Detection

An AI that can detect things typically found in traffic. Examples include cars and trucks.

## The Algorithm

I first create a file full of the labels of the vehicles I'm going to use to no forget.
When I finished, I ran the docker container and went to my detection ssd folder:

    cd ~/jetson-inference/
    ./docker/run.sh

    cd python/training/detection/ssd

Then I downloaded all the photos I needed to train the ai to know what animal is which. I chose to download 5000 photos for this:

    python3 open_images_downloader.py --max-images=2500 \
    --class-names "Bicycle,Bus,Car,Motorcycle,Person,Traffic light,Traffic sign,Truck" \
    --data=data/traffic

After this, I run a coe which makes the ai look over the photos 30 times just to make sure it knows what vehicle is what:

    python3 train_ssd.py --data=data/traffic --model-die=models/traffic --batch-size-4 --epochs=30

Now before scanning any video, I have to run this code to make it possible to actually do it:

    python3 onnx_export.py --model-dir=models/traffic

Lastly, we have to download a video and make the AI scan it for any animal is recognizes, I used this code for it. Depending on the name of the file of the video and what name you want the output of the video to be, you have to change some things (That are mainly naming related):

    detectnet --model=models/traffic/ssd-mobilenet.onnx --labels=models/traffic/labels.txt \
    --input-blob=input_0 --output-cvg=scores --output-bbox=boxes \

I have added two videos as demonstration in the demo folder, if you click on them, you should see the AI I have trained to deteh the sea animals.
