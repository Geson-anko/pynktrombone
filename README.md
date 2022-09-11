# PynkTrombone
The implementation of human vocal organ model. Ported from voc.pdf written in C to Python.   
This repository was created to make dkadish's Python implementation of PinkTrombone faster.  
The functions and modules are about 10x faster with no-python jit.  

<img src=data/trombone.gif width="350px">

---
## Usage
1. Clone this repository and run the following code in this folder to install it.
    ```python
    pip install -e ./
    ```

2. We can import Vocal organ interface (Voc) by following code. 
    ```python 
    from pynktrombone import Voc
    ```

3. Construct Voc 
    ```python
    voc = Voc(Arguments)
    ```  
    main arguments:  
    - samplerate  
    This is the resolution at which it is generated; at 22050 Hz, high-frequency components such as consonants are reduced.  

    - CHUNK  
    The length of the waveform to be generated in one step.

    - default_freq  
    The initial value of frequency of glottis.

    - others  
    please refer voc.py, tract.py and glottis.py  .

4. Modify voc  
    Modify voc using ```voc.tongue_shape(args)``` and etc.
    See demos.py for other items that can be changed.
    
    - Modifiable values of `Voc`  
        - frequency  
            The voice frequency.   
        - tenseness  
            The degree of voice hoarseness.  
            Range [0 - 1]. At `0` is clear.  Higher value gets more hoarser.   
        - `set_tract_parameters` (method)  
            This is a method of Voc class.  
            Following arguments can be modified.  
            -  trachea (throat)  
                The degree of throat opening.  
                Closes at `0`. Maximum opening at about `3.5`.  
            - epiglottis  
                The diameter of it. Range[0 - 3.5].  
            - velum  
                The diameter of it. Range[0 - 3.5].  
            - tongue_index   
                The start position of tongue in the tract.   
                You can change vowel by modifing this value.  
                Range[about 12 - 30?40?]
            - tongue_diameter  
                The size of tongue. Range[0 - 3.5].
            - lips  
                The diameter of lips. Range[0 - 1.5].


5. Let's generate!  
    In the following code, we will change the tongue_diameter and tenseness of the voc in update_fn to generate the voice. there are some examples in demos.py
    ```python
    def play_update(update_fn, filename):
    sr: float = 44100
    voc = Voc(sr)
    x = 0
    voc = update_fn(voc, x)

    out = voc.play_chunk() # modify voc
    while out.shape[0] < sr * 5:
        out = np.concatenate([out, voc.play_chunk()])

        x += 1
        voc = update_fn(voc, x) # modify voc

    sf.write(filename, out, sr)
    ```
---
## Structure of vocal organ.
Please refer voc.pdf about details.   
<img src=data/vocal_tract.png width="320px">