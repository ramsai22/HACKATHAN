# HACKATHAN
# DE-Hackathon
## Step-by-Step Design Approach

### Problem Analysis:

Identify the key factors contributing to traffic congestion (e.g., signal timing, traffic density, emergency vehicle passage).
Determine the areas where digital logic can be applied effectively (e.g., traffic signal control, real-time traffic monitoring, adaptive signal timing).
Requirements Specification:

Design a system that can dynamically adjust traffic signal timings based on real-time traffic data.
Ensure the system can prioritize emergency vehicles.
Implement a method to gather and process traffic density data from different intersections.
System Components and Architecture:

**Sensors:**  Use sensors to detect the number of vehicles at each intersection.

**Counters:** Count the number of vehicles and time spent at each signal.

**Multiplexers:** Select input from different sensors and direct them to a common processing unit.

**Flip-Flops:** Store the current state of traffic lights and transition states.

**Encoders and Decoders:** Convert data from sensors into a form that can be processed by the controller.

### Detailed Design
**1. Traffic Density Measurement**
Sensors and Counters: Use infrared or ultrasonic sensors to detect vehicle presence. Connect these sensors to counters to count the number of vehicles passing or waiting at a signal.

**2. Traffic Signal Control Logic**
State Machine (FSM): Design a finite state machine to control the traffic lights. States will represent different phases of traffic signals (e.g., green, yellow, red).
Flip-Flops: Use flip-flops to store the state of each traffic signal.
Transition Logic: Design combinational logic circuits to determine state transitions based on sensor inputs (e.g., if traffic density is high, extend the green light duration).

**3. Dynamic Signal Timing Adjustment**
Multiplexer: Use a multiplexer to select which intersection's sensor data to process.
Priority Encoder: Implement a priority encoder to prioritize emergency vehicles. If an emergency vehicle is detected, it overrides the normal traffic signal sequence.
Timing Control: Use counters to manage the duration of each traffic light phase. The counter values can be dynamically adjusted based on traffic density.

**4. Integration and Control**
Controller Unit: Design a central controller unit (using a microcontroller or FPGA) to integrate all the components. The controller will read sensor data, process it, and output control signals to the traffic lights.
RTL Schematic: Create the RTL schematic to visualize and simulate the circuit design before implementation.

## Implementation and Testing
### Simulation:

Use a digital simulation tool (e.g., ModelSim, Xilinx Vivado) to simulate the traffic control circuit.
Test various traffic scenarios to ensure the circuit responds correctly to changes in traffic density and emergency vehicle presence.
### Prototype Development:

Implement the design on a hardware platform (e.g., FPGA board).
Connect the sensors and traffic lights to the FPGA and test the real-time performance of the system.
### Validation and Optimization:

Validate the system in a real-world or simulated traffic environment.
Optimize the signal timing algorithms and hardware configuration based on test results to improve efficiency and reduce congestion.

## PROGRAM
**Developed by :** PAIDA RAM SAI

**Register No :** 212223110034



module TrafficLightController(
    input clk,
    output reg red1,
    output reg yellow1,
    output reg green1,
    output reg red2,
    output reg yellow2,
    output reg green2,
    output reg red3,
    output reg yellow3,
    output reg green3
);

// State machine definition
parameter S_IDLE = 2'b00;
parameter S_ROAD1 = 2'b01;
parameter S_ROAD2 = 2'b10;
parameter S_ROAD3 = 2'b11;

reg [1:0] state;
reg [3:0] count;

always @(posedge clk) begin
    // State transition
    case(state)
        S_IDLE: begin
            count <= count + 1;
            if (count == 5) begin
                state <= S_ROAD1;
                count <= 0;
            end
        end
        S_ROAD1: begin
            count <= count + 1;
            if (count == 5) begin
                state <= S_ROAD2;
                count <= 0;
            end
        end
        S_ROAD2: begin
            count <= count + 1;
            if (count == 5) begin
                state <= S_ROAD3;
                count <= 0;
            end
        end
        S_ROAD3: begin
            count <= count + 1;
            if (count == 5) begin
                state <= S_IDLE;
                count <= 0;
            end
        end
    endcase
end

// Traffic light control logic
always @(*) begin
    case(state)
        S_IDLE: begin
            red1 = 1;
            yellow1 = (count >= 1 && count <= 4) ? 1 : 0;
            green1 = 0;
            red2 = 1;
            yellow2 = 0;
            green2 = 0;
            red3 = 1;
            yellow3 = 0;
            green3 = 0;
        end
        S_ROAD1: begin
            red1 = 0;
            yellow1 = (count >= 6 && count <= 9) ? 1 : 0;
            green1 = (count >= 1 && count <= 5) ? 1 : 0;
            red2 = 1;
            yellow2 = (count >= 6 && count <= 9) ? 1 : 0;
            green2 = 0;
            red3 = 1;
            yellow3 = 0;
            green3 = 0;
        end
        S_ROAD2: begin
            red1 = 1;
            yellow1 = 0;
            green1 = 0;
            red2 = 0;
            yellow2 = (count >= 1 && count <= 4) ? 1 : 0;
            green2 = (count >= 6 && count <= 9) ? 1 : 0;
            red3 = 1;
            yellow3 = (count >= 6 && count <= 9) ? 1 : 0;
            green3 = 0;
        end
        S_ROAD3: begin
            red1 = 1;
            yellow1 = 0;
            green1 = 0;
            red2 = 1;
            yellow2 = 0;
            green2 = 0;
            red3 = 0;
            yellow3 = (count >= 1 && count <= 4) ? 1 : 0;
            green3 = (count >= 6 && count <= 9) ? 1 : 0;
        end
    endcase
end

endmodule

## RTL SCHEMATIC DIAGRAM : 

![image](https://github.com/PuliNagaNeeraj/DE-Hackathon/assets/138849173/9147921c-16c8-4221-a1e3-ed9bc93164e8)

## WAVEFORM :

![image](https://github.com/PuliNagaNeeraj/DE-Hackathon/assets/138849173/418c8ddb-4aaf-4a51-94d7-6071c8684baa)

## RESULT : 
The above Verilog code implements a traffic light controller for a three-road intersection. The system cycles through states, each representing different roads having green, yellow, or red lights, while ensuring synchronization of traffic signals. The hackathon top-level module integrates the traffic light controller, enabling efficient traffic management and reducing congestion by systematically controlling the light transitions based on a state machine.
