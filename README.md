# Poem Generation 

## Abstract
Poetry is a literary work in which special intensity is given to the expression of feelings and ideas by the use of distinctive style and rhythm. However, generating high-quality poems that preserve a given rhyming pattern using a minimal amount of computing power remains a challenge. For our final project, we present an approach leveraging Long Short-Term Memory (LSTM) networks and a custom rhyme-constrained beam search algorithm to generate poetry that preserves a specific rhyming pattern. The results have shown that using a simple LSTM architecture enables the generation of meaningful poetry which paired with a rhyme-constrained beam search algorithm can preserve the rhyming pattern of the poems. This means that it is possible to generate rhyming poetry without the use of computationally expensive methods.

## Dataset
As the project is centered around the rhyming poetry form, we carefully selected a dataset that supports our objectives. The primary criteria for dataset selection were a substantial volume of text and adherence to a specific rhyming and structural pattern. After evaluating several sources, we chose The OEDILF dataset, an online collaborative project that aims to define all English words through limericks. This choice was motivated by the unique traits of limericks which strictly follow the distinctive AABBA rhyming pattern, maintain a consistent five-line format, and are written in an easy to understand language making it ideal for syntactic and semantic modeling in our project.
 
### Dataset Specifications
- Total Number of Poems: 98,601
- Total Number of Words: 2,882,152
- Total Number of Characters: 16,339,587
- Source URL: http://www.oedilf.com/db/Lim.php 
 
## Implementation
During inference we utilize the beam search algorithm that is designed to explore multiple hypotheses in parallel, retaining only the top-k most promising candidates at each step, thereby improving upon the greedy approach of considering only the most likely next token.

![beam-search](https://github.com/Aslanbayli/poem_generation/assets/48028559/bf771b8e-7131-4afa-b295-a8fcde2afd1e)

*credit: https://d2l.ai/chapter_recurrent-modern/beam-search.html*

The logic for enforcing a rhyming structure is fairly simple, we generate the first line using the traditional beam search. Then, based on the last three characters of the first line, which is the matching criteria we decided to use, we pick the last word for the next line that is most likely to rhyme with them. The rhyme word for every subsequent line is based on the previous and uses the same logic. To find which word is most likely to rhyme with the given last three letters, we use a lookup table which contains potential rhyming pairs and their probabilities constructed during the data pre-processing step.

It is important to note that the rhyming mechanism is not merely replacing the last words of the lines to create rhymes. Instead, at each step of the beam search, the rhyme matches are identified, and the next line is generated by taking into account the last word of the preceding line, ensuring a coherent and seamless rhyming scheme throughout the poem.

## Results

![image](https://github.com/Aslanbayli/poem_generation/assets/48028559/f6a390cf-93f9-400a-a340-6066a2faed39)

The implemented character-level language model based on an LSTM architecture has shown promising results in generating limerick-style poems. The model was trained on a limerick dataset with a vocabulary of 86 unique characters, and the training process involved splitting the data into train, validation, and test sets.

The hyperparameters of the model, including batch size, learning rate, number of RNN units, and embedding dimensions, were tuned using a grid search approach. The best configuration found was a batch size of 64, a learning rate of 0.001, 512 RNN units, and an embedding size of 512. This configuration achieved a final training loss of 1.3415 and a validation loss of 1.3346 after 30 epochs of training.

The training and validation losses were monitored throughout the training process, and the model did not exhibit signs of significant overfitting or underfitting. The validation accuracy improved from approximately 48% after the first epoch to around 58% after 30 epochs, indicating that the model learned to capture the patterns in the limerick dataset. 

To generate new limerick poems, a beam search decoding strategy was employed, considering rhyme patterns and enforcing the limerick structure. The generated limericks demonstrate the model's ability to produce coherent and rhyming verses, although the semantic quality and creativity of the generated poems may require further improvements.

Overall, this word-level language model shows potential in the task of limerick poem generation and could potentially be extended to other forms of poetry or creative writing tasks. Future work could involve exploring more advanced model architectures (for example, Bi-directional LSTM with an attention layer), incorporating additional linguistic features (and maybe different poem genres), or fine-tuning the model on larger and more diverse datasets to enhance its generative capabilities.

