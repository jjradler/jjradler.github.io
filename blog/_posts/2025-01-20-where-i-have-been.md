---
layout: post
title: Where Have I Been? What Have I Been Doing? 
Category: Career
tags: [ career, future, pedagogy, teaching, learning, tools, life changes, academia, industry, updates ]
description: >
    A brief discussion of where I have been and where I hope to go next. 
---

## Graduate School:

In early 2019, I left my graduate studies in computational chemistry after four years of study and intense research. The outcome was due to the vagaries of the American science PhD environment, funding, and the overall structure and incentives of Academia at the time.

Since my research topic (mixed quantum-classical dynamics theory and simulation) was rather niche and difficult to pursue due to the very long simulation times and heavy computing resource requirements, it was not feasible (or even really possible) for me to simply transfer to another research group and continue my studies, although I did pursue discussions with other professors. 

I think everyone involved was doing the best they could with the systems that were in place at the time. 

I do not want to wade into the current incentive structures of American Academia, the very strange and stressful environment of pursuing a science PhD, and their effects on the Academy as a whole as I am not sure it would add to the discussion in any meaningful way. I do have my own opinions and conjectures and I have heard many other anecdotal stories from other aspiring academics that sound remarkably similar to mine, so overall my exit from my PhD program is not somehow unique. However, it does indicate that one’s *technical* performance in the Academic space is not a guarantor of success.

That being said, I did have a wonderful time in graduate school between the stressful moments. I made great friends. I learned a lot about communication with colleagues and how different people convey and process information. I learned about the problem I was working on in great depth. Some of my writings on the other parts of this very site are explorations of the theories underpinning my research problem.  

I feel fortunate to have had the opportunities I did to publish, present my work, win awards, interact with giants in the computational chemistry field, and contribute to the body of human knowledge in peer-reviewed journals. I am only really saddened by realizations of the difficulties and (financial costs) a curious, skilled, but otherwise unaffiliated researcher has when it comes to accessing articles, computing resources, or even becoming a part of the conversations being had in research spaces without the direct support of an institution or company. 

## Teaching:

After exiting the PhD program, I had a short stint as a full-time Teaching Associate at the University of Washington. I taught General Chemistry to hundreds of students and mentored others over four years while I was there and deeply enjoyed the experience. I even had the opportunity to lead a group of TAs in the Chemistry department as a “Lead TA”. The Lead TA role still taught undergraduate students, but it had more organizational and administrative components as it was intended to take some of the day-to-day operational workload off of the (very hardworking) Lecturers.  

I hope some of my students look back at our time together fondly the same way I remember some of the excellent, supportive, and engaged instructors from my journey through higher education. 

## Entering Industrial Research & Development:

Once the Spring Quarter of 2019 ended, my Teaching Associate contract had ended and I was now on the job market. 

At the time, opportunities for Computational or Theoretical Chemists were very limited in number and explicitly restricted to candidates with PhDs. I applied for a number of roles anyway, but not hearing back I soon broadened my search to include jobs that could use my scientific computing, data science, and signal processing skills more broadly. Technical problems are wonderful, but I did also need to pay my bills, and would have preferred a role in scientific computing with which to do so.  

Soon, I was recruited for a job doing digital signals processing for a small, early-stage startup that had a very compelling problem:  they needed to interpret noisy, complex, dynamic signals from an underwater survey apparatus intended for detecting petroleum and metal contamination in the seabed. The whole platform was designed to be used as a tool for quickly locating hotspots where teams doing site cleanup from oil spills or performing offshore salvage operations should focus their efforts. One of the most time consuming and expensive aspects of site cleanups for spills or wrecks is identifying which portions of the cleanup area have the most contamination and therefore need the most attention from the cleanup teams. 

I had found a niche!

As it turns out, my studies of wave mechanics, coherences, and time-frequency analysis were very applicable to interpreting the changes to analog waveforms used by their device that indicated the electrochemical “anomalies”. “Anomalies” is the term used to describe capacitive or inductive effects on the signal transmitted by the device that should theoretically highlight the contaminated spots in the seabed. Since the device was dragged behind the surveying vessel at a specific velocity, the time-varying signals were a critical part of the detection technology. 

*I will not go into much detail about the technology stack here as the company is still performing work in this area and much of it is still proprietary.*

I stayed at that job for five years because I really believed in the technology and felt I could do a lot of good to advance the technology.

I had to be very self-directed and quickly become an expert on both the underlying physics and the engineering of the machine. I would go so far as to suppose that this degree of autonomy and self-direction may only be found in startup environments, and I loved it. Being a specialized space, custom tools and solutions were needed. Even working out *what* to build required synthesizing paths and solutions from various different sources in the research phase, followed by rapid development, construction, and iteration on possible solutions.

It was very stimulating, dynamic, and was a great place to keep my skills sharp.  

Doing the job required applying Python and embedded C programming extensively to implement everything from digital-to-analog signal generators for the device transmitters to time-frequency analysis and experiments with machine-learning techniques for interpreting the multi-channel, coupled analog signals returned to the device from its sensors. My visualization skills were frequently developed and sharpened in the role because long surveys with up to eight channels of time-varying, coupled data – including spectral analysis – needed to be interpreted for stakeholders at a glance if possible. Using the techniques I had learned by doing scientific computing and High-Performance Computing (HPC) in graduate school, I was able to perform and improve the automation and data management capabilities of the technology stack. This was necessary because multiple days of surveys could generate tens of gigabytes of text data – all of which needed organization, labeling, and storage for later postprocessing and deeper analysis.

In addition to the software development and data analysis aspects of the job, a situation arose halfway through my tenure at the company where we needed to build a new set of prototype devices. I had been working with analog synthesis for audio, embedded processors, and guitar effects circuits intensely for about a year as a hobbyist, so I volunteered to take on the design and development of the new hardware stack in addition to the software and firmware components needed to operate it in the field. I quickly ramped up my skills in electronics design, bench-testing, component evaluation, schematic capture, and printed circuit board (PCB) design. I also had to select the toolchains, development platforms, and protocols needed to interface the machine with the operators’ control and navigation systems. 

It turns out, many of the skills needed to design a PCB still boil down to understanding models of electromagnetic waves and thermal properties of matter, just like what I had studied in graduate school – albeit with coarser resolution and more handy approximations! 

Within two years of starting the redesign, we had a working prototype system that was custom designed and purpose-built for its application, complete with custom firmware for generating transmitter signals and custom circuitry for safely and accurately handling the analog return signals from the seabed. 

I believe deeply that this technology has the potential to revolutionize disaster cleanup and environmental management. Unfortunately, despite the successes and all the advancement in the technology, economic and logistical factors forced the company to lay me off midway through 2024. The downside of this is, of course, that I am no longer actively involved with developing that technology that I had invested a great deal of time and energy into. However, no longer working at a startup does free up a *lot* of my cognitive and energy bandwidth that I can use on contributing to open-source scientific computing projects as well as my own development projects within computational chemistry, cheminformatics and high performance computing. 

## Independent Consulting:

Since leaving my developement job at the startup, I have been applying for more computational chemistry, quantum computing, and scientific computing jobs while working as an independent technical consultant. My projects for clients right now have included designing, building, testing, and delivering an embedded, digitally controlled ultrasonic signal generator for defouling industrial equipment and an associated sensor system to go with it. I will not discuss this in detail as, again, the technology is proprietary. 

## The Future:

The good news is, it seems that the Computational Chemistry and Quantum Computing spaces are opening up to more individuals with skills in those areas *without requiring PhDs*, so I am hopeful that I can get myself back into doing deep research and development work in the areas within which I have continued to study and develop skills. 

Accordingly, now that I have a bit more free time and energy, I have the ability to contribute to the many interesting, open-source tools that have appeared in the space since I left Academia. 

These days, previously difficult fields to work in without institutional support like High-Performance Computing are more open than ever to skilled practitioners who are unaffiliated with univerisities or large companies. Computing hardware is faster and cheaper than it has ever been. Cloud computing seems much more accessible for an individual researcher, and now there seems to be a plethora of affordable – and even “free as in freedom” – open-source tools for accessing quantum computers and administrating small scale HPC clusters.

I am excited to explore other areas that I have not have the opportunity to pursue yet like cheminformatics and the integration of quantum computing with molecular simulations. I now have the time to learn some new programming languages and play with tools like `RDKit`, `qiskit`, `PySCF`, and others while I apply for more upcoming computational chemistry or quantum computing roles.

