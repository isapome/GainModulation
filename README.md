# GainModulation

## Notes

### Jan 4, 2020

Trying to implement attention as a neuronal gain modulation mechanism. 

Code not so flexible, needed major revision. 

Current version can take CIFAR10 and MNIST as datasets and various architectures.

It seems like using only 1K samples out of 60K for validation might be not significant. Training accuracy on MNIST with 3 dense HLs (1500-1000-500) stays below 99% but validtion accuracy goes to 100%. Currently taking almost 1m/epoch. (Neuronal gain updates/batch == 1, batch size training == 1, batch size testing ==1).

Using the test data as validation data (i.e. 10K samples) the validation accuracy goes on par with the training accuracy. It never reaches 98.5%.

I think it is possible to calculate an average of the attentional update over a mini-batch. 

NB with attention span == 0 the code does not work. Why? Shouldn't it be the same as a normal weight update?

Found maybe something: why when doing the backward pass the feedback signal from a layer to another is not simply (weight * derivative) but (new_weight * derivative) with new_weight = layer_input * attention / activation ??? (activation being sum(w * input)+b). I have changed it to ffweights and it gives the same results as before. Maybe with deeper networks the effects would be more evident?


### Jan 5, 2020

The attentional gain can be a [batch_size, neurons] matrix instead of a [neurons] vector. This way, it might be possible to do mini batches learning. However, we might need to check how the weight update is done, since it is still not working properly.
