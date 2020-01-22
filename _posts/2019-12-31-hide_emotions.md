---
layout: post
title: Hiding emotions in plain sight
---
# Summary of the project
With the prevalence of intelligent personal digital assistance devices like amazon Alexa, Siri andGoogle home, Privacy has become a greater concern. This is because these devices listen to the user(and other people around them) and process these vocal data and make inferences. For example these devices may learn your identity and emotional state. Some researchers are calling for a ban for emotion recognition technologies because they can be biased (e.g racially). Also it may listen to confidential and private conversations you are having. The threat becomes greater since these devices usually send the data (voice recordings) to cloud for further analysis. Some malicious agent may get hold of these data. Therefore it is important to investigate methods to mask certain attributes out from voice. For example, if we can hide the emotional content from voice, and if we can still use other voice functions (such as speech recognition), it would be beneficial

# Main idea
Consider a personal digital assistant like Amazon Alexa.
1. Find a method where we can hide our emotional content from speech signal
2. But the digital assistant should be able to use this modified voice and still extract other information (e.g. convert the speech to text)
3. State-of-the-art emotion recignition systems use CNNs and use magnitude spectrograms as their input.
The work flow is :
voice -> spectrogram -> CNN -> emotion_prediction 
4. We can use the method called Fast Gradient Sign Method (FGSM) to create adversarial samples. This means we can use FGSM to modify spectrograms so that a state-of-the-art emotion recognition system cannot classify the emotion we elicit using this modified spectrogram. 
5. I saw that after modifying the spectrogram, the model cannot classify emotions with a good accuracy level. But speech-to-text function (I used an off the shelf speech-to-text from python) works without a problem.
6. But there is an issue with this approach. We cannot isolate the spectrogram or any other features generated in a modern digital assistant. We just have access to the voice signal.
7. I extended this mehtod so I can directly modify the voice signal with FGSM instead of modifying the spectrograms. So this would be practically applicable. 
8. After these modification, the model fails to classify emotions in a reliable manner. But speech-to-text still works as expected.
9. Thereofre we can see that this method can be applied in practical scenarios. 

Report of the project I did: 
[get the PDF](/files/hide_emotions.pdf).

[project repo](https://github.com/sleekEagle/hide_emotion)
















