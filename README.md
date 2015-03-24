# Microprocessor-Simulator
Programs in smz32v50

# 1

Text input and display program with procedures.
Use one procedure to input the text and one to display it



     THE MAIN PROGRAM
        MOV     BL,70   ; [70] is the address where the text will be stored. 
        MOV     DL,C0   ; Makes DL point to VDU Screen

        CALL    10      ; reads in text and places it starting from the address in BL.

        CALL    40      ; displays text on VDU Screen


     PROCEDURE TO READ IN THE TEXT
        ORG     10      ; Code starts from address [10]

        PUSH    AL      ; Save AL onto the stack
        PUSH    BL      ; Save BL onto the stack
        PUSHF           ; Save the CPU flags onto the stack

     Rep:
        IN      00      ; Input from port 00 (keyboard)
        CMP     AL,0D   ; Was key press the Enter key?
        JZ      Stop    ; If yes then jump to Stop
        MOV     [BL],AL ; Copy keypress to RAM at position [BL]
        INC     BL      ; BL points to the next location.
        JMP     Rep     ; Jump back to get the next character

    Stop:
        MOV     AL,0    ; This is the NULL end marker
        MOV     [BL],AL ; Copy NULL character to this position.

        POPF            ; Restore flags from the stack
        POP     BL      ; Restore BL from the stack
        POP     AL      ; Restore AL from the stack

        RET             ; Return from the procedure.

     PROCEDURE TO DISPLAY TEXT ON THE SIMULATED SCREEN
        ORG     40      ; Code starts from address [10]
     JOHN:
        MOV     CL,[BL] ; Copies RAM [70] to CL
        MOV     [DL],CL ; Copies CL to VDU Screen
        INC     DL      ; Move to next VDU Space
        INC     BL      ; Moves to next RAM spot
        CMP     CL,00   ; Is CL 00(Enter)? If yes finish procedure
        JNZ     JOHN    ; If not jump to JOHN
        RET             ; Return from the procedure



     END           ; End of program
# 2
This Program is a working Traffic Light.
     
	CLO		; Close unwanted windows.
     Loop:
     		; Turn on Red and Green lights.
	MOV AL,84	; Copy 10000100 into the AL register.
	MOV BL,8    	; Count for timer
	Out 01 		; Send AL to Port One (Traffic light simulator).
	CALL	30	; Call the procedure at address [30]

			; Turn on Red and Yellow lights.
	MOV AL,88	; Copy 10001000 into the AL register.
	Out 01 		; Send AL to Port One (Traffic light simulator).
	MOV BL,3     	; Count for timer
	CALL	30	; Call the procedure at address [30]

			; Turn on Green and Red lights.
	MOV AL,30	; Copy 00110000 into the AL register.
	Out 01		; Send AL to Port One (Traffic light simulator).
	MOV BL,8    	; Count for timer
	CALL	30	; Call the procedure at address [30]

			; Turn on Yellow and Red lights.
	MOV AL,50	; Copy 01010000 into the AL register.
	Out 01 		; Send AL to Port One (Traffic light simulator).
	MOV BL,3     	; Count for timer
	CALL	30	; Call the procedure at address [30]

	JMP Loop	; Jump program back to the start.


       Time Delay Procedure   	
	ORG	30	; Generate machine code from address [30]

	PUSH	BL	; Save AL on the stack.
	PUSHF		; Save the CPU flags on the stack.
     
     Rep:
	
	DEC	BL	; Subtract one from AL.
	JNZ	REP	; Jump back to Rep if AL was not Zero.

	POPF		; Restore the CPU flags from the stack.
	POP	BL	; Restore AL from the stack.

	RET		; Return from the procedure.

	END		; Program ends
