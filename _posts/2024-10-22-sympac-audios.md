---
layout: post
title: "SymPAC Audio Demos"
comments: true
date: "2024-10-22"
description: ""
keywords: "music, music ai, symbolic music, audio, mir, music information retrieval, generation, language model, music generation, demo, sympac"
---

Audio demos of [our paper](https://arxiv.org/abs/2409.03055) about symbolic music generation. The symbolic representation contains 5 stems (vocal, piano, guitar, bass, drums). Vocal stem is rendered using bell sound.

<table style="border-collapse: collapse;">
    <tr>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/01.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/02.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
    </tr>
    <tr>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/03.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/04.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
    </tr>
    <tr>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/05.themerepeat.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/06.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
    </tr>
    <tr>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/07.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/08.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
    </tr>
    <tr>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/09.keychange.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
        <td>
          <audio controls>
            <source src="../../assets/files/20241022/10.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
        </td>
    </tr>
</table>

This model is trained on a music audio dataset with mostly Chinese pop songs. We transcribe the audio into symbolic domain using MIR models (beat tracking, chord detection, structure analysis, 5-stem transcription and music tagging). The outputs of MIR models are then transformed into a token sequence to train a transformer-based language model.

Some characteristics we think are interesting:
* The model is able to keep the theme over the entire song, showing strong ability to model long-term dependencies.
* Some generated pieces have key shift halfway, with quite smooth transitions (e.g. two samples on the last row).

Our model is also able to take control inputs, such as chord progression and song structure. Below is a piece generated with uncommon chord progression (looping major keys A-B-C-D-E-F-G-A).
<p>
<audio controls>
  <source src="../../assets/files/20241022/chord_input_A-B-C-D-E-F-G-A.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>
<p>
Although the input chord progression is quite uncommon, the model is still able to find a balance between following the input and producing a reasonable melody, which surprises us.