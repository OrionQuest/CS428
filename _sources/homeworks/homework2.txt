Homework #2
===========

For all programming assignments, please turn in your code along with your
solution. For all hardware assignments, please provide as much details on your
implementation, along with a video of the final setup in action. Submissions should be made on Sakai,
and must include all material in a single ZIP file with all descriptions/code, and a video file.

.. topic:: Problem 1

    Write an OpenGL program to load a 3D humanoid model and display it to screen. Implement controls for lifting it's arms and legs
    using key bindings. Note that there should be a separate control for the upper/lower left and right arms (and similarly for the legs).
    When the upper arm is moved, the lower arm should move as well, analogous to how the human body moves.


.. topic:: Problem 2 (**Intruder Alarm**)

    Implement a circuit that uses an ultrasonic sensor to measure distance and display the output to an LCD screen.
    If the distance is below a specified threshold, then a buzzer should start beeping, indicating that an intruder is trespassing.
    When the intruder exits, the buzzer should stop beeping.

    **Extra Credit:** Mount the ultrasonic sensor on a Servo motor to scan the front 180 degrees. If an intruder is inside a specified
    radius to the ultrasonic sensor, the Servo should stop scanning and point directly at the intruder, and the buzzer should start beeping.
    When the intruder exits, the Servo should start scanning again, and the buzzer should stop beeping.
