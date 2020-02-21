FORK:
https://github.com/rewicks/ParlAI/
 
MODEL:
stored on the coe grid (let us know if you don't have access to it) at
/exp/rwicks/ParlAI/movietriples_2-19/single.model.{checkpoint, checkpoint.dict, checkpoint.dict.opt}

==================================
REPRODUCTION STEPS:
1. create an alexa app in the developer console
2. copy the manifest.json into the json editor on the developer console
3. run ./ngrok
4. run python lambda_function.py
5. copy the https from ngrok into the alexa endpoint
6. choose the second option from the dropdown
7. talk to alexa

for interaction (no alexa):

1. copy the model directory from /exp/rwicks/ParlAI/movietriples_2-19/ on the coe grid
2. run 'python examples/eval_model.py -mf movietriples_2-19/single.model.checkpoint -t movietriples -dt test'



==================================
EVALUATION:

TEST
{'exs': 48542, 'accuracy': 0.005500391413621194, 'f1': 0.0868729140744965, 'bleu-4': 0.00040769832556888984, 'total_train_updates': 0, 'gpu_mem_percent': 0.103, 'loss': 190500.0, 'token_acc': 0.3226, 'nll_loss': 4.132, 'ppl': 62.33}

VALID
{'exs': 49434, 'accuracy': 0.0055832018448840875, 'f1': 0.0872162364614846, 'bleu-4': 0.0005406841956268879, 'total_train_updates': 0, 'gpu_mem_percent': 0.0876, 'loss': 196500.0, 'token_acc': 0.3219, 'nll_loss': 4.164, 'ppl': 64.32}

We didn't add another evaluation metric. sorry bout that.

==================================
Issues with the HRED agent:

1.  It only keeps track of a 2-length context-- that is, its own previous utterance
	and the most recent utterance from the other speaker to respond to. This is just
	an issue with the implementation, and could be rectified by storing an infinitely
	long sequence of utterances in the parlai agent history to be passed to the forward()
	function of the HRED model.
2.  (related) It can get stuck in infinite loops in which it only responds with a generic
	response-- e.g. "i 'm sorry" or "i don 't know". Extending the context and implementing
	reverse LMing should both help with this.
3.  We did not handle outputted __unk__ nor <person> placeholders.
4.  We attempted to append dailydialog interactions (preprocessed into dialog triples) to the
	movietriples dataset, but it resulted in a much worse model that doesn't say anything
	except "i 'm sorry ." This might be because we removed previous utterances from contexts
	to make them conform with the triplet format. Increasing context length should also help
	with this? (Dailydialog was still far superior to our augmented data--carlostriples)

Future work:

1.  increasing context
2.  adding the reverse LM objective
3.  adding other datasets
4.  hyperparameter search during training

==================================
Example interactions:

EXAMPLE 1 ------------------------

Enter Your Message: this is a test .
[HRED]: so you ' re going to have to do something about it ?
Enter Your Message: no , we're going to submit you .
[HRED]: <person> , i ' m going to get you a job .
Enter Your Message: what kind of job ?
[HRED]: i ' ll take a job .
Enter Your Message: what job ?
[HRED]: i ' m not a job .
Enter Your Message: what are you ?
[HRED]: i ' m a little girl .


EXAMPLE 2 ------------------------

Enter Your Message: Rachel give me a test utterance .
[HRED]: <person> , i ' m a little bit .
Enter Your Message: what is a little bit?
[HRED]: i don ' t know . i ' m just a little .
Enter Your Message: a litte what ?
[HRED]: i like to talk to her .
Enter Your Message: who  ?
[HRED]: <person> .
Enter Your Message: why ?
[HRED]: i ' m not sure .
Enter Your Message: you 're not very smart .
[HRED]: i ' m not .

EXAMPLE 3 ------------------------

Enter Your Message: I look and feel like a midwestern housewife making a 3am diaper run to Meijer !
[HRED]: you ' re the one who ' s not the one who ' s been to the one .
Enter Your Message: that is very deep .
[HRED]: so . it ' s not your fault .
Enter Your Message: thank you .
[HRED]: i ' m sorry .
Enter Your Message: why ?
[HRED]: i ' m sorry .
Enter Your Message: why are you sorry ?
[HRED]: i ' m sorry .

EXAMPLE 4 ------------------------

Enter Your Message: what do you think of the israel palestine conflict ?
[HRED]: you think i ' ve got a good idea ?
Enter Your Message: yes, i  do .
[HRED]: <person> , i ' m a little too bad .
Enter Your Message: i believe in you .
[HRED]: i ' m going to miss you .
Enter Your Message: why ?
[HRED]: i ' m going to kill you .
Enter Your Message: that is dark .
[HRED]: so what are you gon na do ?
Enter Your Message: turn you off ...
[HRED]: she ' s gon na get me a drink .
Enter Your Message: you can't drink . it's 1 in the afternoon .
[HRED]: i ' m not sure .

EXAMPLE 5 ------------------------

Enter Your Message: i have a very particular set of skills .
[HRED]: i ' m a little strong .
Enter Your Message: skills that make me a nightmare for people like you .
[HRED]: i ' m not a man . i ' m a good man .
Enter Your Message: is that supposed to be profound ?
[HRED]: i know . but you ' re not the one who ' s not .
Enter Your Message: that was hurtful .
[HRED]: that ' s what i ' m asking .
Enter Your Message: what are you asking ?
[HRED]: the way you ' re doing is i ' m not sure .
Enter Your Message: excuse me ?
[HRED]: i ' m not sure i ' m going to do it .
Enter Your Message: do what ?
[HRED]: i ' m not going to do anything .
Enter Your Message: ok .
[HRED]: you ' re not going anywhere .
Enter Your Message: yes i am .
[HRED]: and i ' ll tell you something .
Enter Your Message: what ?
[HRED]: i ' m not sure .

EXAMPLE 6 ------------------------

Enter Your Message: do you want to kill me ?
[HRED]: no .
Enter Your Message: good .
[HRED]: i ' m sorry .
Enter Your Message: for what ?
[HRED]: i ' m sorry .
Enter Your Message: this seems like an infinite loop .
[HRED]: what are you saying ?
Enter Your Message: you 're a model . and you 're stuck in an infinite loop .
[HRED]: you got ta get me a job .
Enter Your Message: submit your resume to indeed .
[HRED]: i ' m not a fucking liar .

EXAMPLE 7 ------------------------

Enter Your Message: where did you come from ?
[HRED]: <person> .
Enter Your Message: where did you go ?
[HRED]: i don ' t know .
Enter Your Message: where did you come from, cotton eye joe ?
[HRED]: i ' ve been in a lot of trouble .
Enter Your Message: what kind of trouble ?
[HRED]: i don ' t know .
Enter Your Message: don 't be coy with me .
[HRED]: <person> , i ' m not .
Enter Your Message: yes you are .
[HRED]: no , i ' m not .
Enter Your Message: liar .
[HRED]: i ' m not lying .
Enter Your Message: how do you know ?
[HRED]: i know .
Enter Your Message: no , you don 't .
[HRED]: i know .
Enter Your Message: liar .
[HRED]: i ' m sorry .

