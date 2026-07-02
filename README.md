# Traffic-Detection

An AI that can detect things typically found in traffic. Examples include cars and trucks.

## The Algorithm/How It Works

Firstly, create a file full of the labels you're going to use to not forget.
When finished, run the docker container and go to your detection ssd folder:

    cd ~/jetson-inference/
    ./docker/run.sh

    cd python/training/detection/ssd

Then, download all the photos you need to train the AI to know what vehicle is which. I personally chose to download 5000 photos for this, but you can choose any amount as long as it's reasonable:

    python3 open_images_downloader.py --max-images=2500 \
    --class-names "Bicycle,Bus,Car,Motorcycle,Person,Traffic light,Traffic sign,Truck" \
    --data=data/traffic

After this, run the code which makes the AI look over the photos 30 times to make sure it knows what vehicle is what:

    python3 train_ssd.py --data=data/traffic --model-die=models/traffic --batch-size-4 --epochs=30

Now before scanning any video, run this code:

    python3 onnx_export.py --model-dir=models/traffic

Lastly, download a video and make the AI scan it for any vehicle it recognizes. Depending on the name of the file of the video and what name you want the output of the video to be, you may need to alter some things:

    detectnet --model=models/traffic/ssd-mobilenet.onnx --labels=models/traffic/labels.txt \
    --input-blob=input_0 --output-cvg=scores --output-bbox=boxes \

I have added two videos as demonstration in the demo folder, if you click on them, you should see the AI I have trained to detect the vehicles.
