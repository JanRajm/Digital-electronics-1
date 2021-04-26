# Computation of avarage speed

## avg_velocity.vhdl
```vhdl
----------------------
-- Made by Jan Rajm --
----------------------
library ieee;
use IEEE.math_real.all;                 -- Needed for power
use ieee.std_logic_1164.all;            -- Basic library
use ieee.numeric_std.all;               -- Needed for shifts

entity e_avg_velocity is
    Port (
        clk          : in std_logic;
        reset        : in std_logic;
        velocity     : in std_logic_vector(7-1 downto 0);
        avg_velocity : out unsigned(12-1 downto 0)    
    );
end e_avg_velocity;
 
architecture behave of e_avg_velocity is

  signal sum_of_velocities     : unsigned(12-1 downto 0) := "000000000000";
  
begin
 
p_avg_velocity : process(clk, reset)

  variable count_of_shifts     : integer    := 0;
  variable clk_cycles          : integer    := 0; -- states for how many clock cycles does signal sum_of_velocities adds samples of 
                                                  --   velocity and for how long does process wait for division
    begin
    
    if rising_edge(reset) then
        avg_velocity      <= "000000000000"; -- zeroing value of avarage velocity
        sum_of_velocities <= "000000000000"; -- zeroing of sum of velocities
        count_of_shifts := 1;
        clk_cycles      := 2; -- shift of one bit correspondents with waiting for two samples os velocity to be summed
    else
        if rising_edge(clk) then 
                    
            sum_of_velocities <= sum_of_velocities + unsigned(velocity);             
                
            if clk_cycles = 0 then
                avg_velocity <= shift_right(unsigned(sum_of_velocities), count_of_shifts); -- here comes division of summed velocities 
                count_of_shifts := count_of_shifts + 1;                                    --     in order to compute avarage velocity
                clk_cycles := 2**(count_of_shifts - 1); 
            end if;
        
            clk_cycles := clk_cycles - 1;        
          
        end if;
    end if;
  end process p_avg_velocity;
end architecture behave;
```
## tb_avg_velocity.vhdl
```vhdl
------------------------------------------------------------------------
--
-- Testbench for N-bit Up/Down binary counter.
-- Nexys A7-50T, Vivado v2020.1.1, EDA Playground
--
-- Copyright (c) 2020-Present Tomas Fryza
-- Dept. of Radio Electronics, Brno University of Technology, Czechia
-- This work is licensed under the terms of the MIT license.
--
------------------------------------------------------------------------

library ieee;
use ieee.std_logic_1164.all;

------------------------------------------------------------------------
-- Entity declaration for testbench
------------------------------------------------------------------------
entity tb_cnt_up_down is
    -- Entity of testbench is always empty
end entity tb_cnt_up_down; 

------------------------------------------------------------------------
-- Architecture body for testbench
------------------------------------------------------------------------
architecture testbench of tb_cnt_up_down is

    -- Number of bits for testbench counter

    --Local signals
    signal s_clk          : std_logic;
    signal reset          : std_logic;
    signal s_velocity     : std_logic_vector(7-1 downto 0);
    signal s_avg_velocity : std_logic_vector(12-1 downto 0);

begin
    -- Connecting testbench signals with cnt_up_down entity
    -- (Unit Under Test)
    uut_avg_vel : entity work.e_avg_velocity
        
        port map(
            clk                            => s_clk,
            velocity                       => s_velocity,
            reset                          => reset,
            std_logic_vector(avg_velocity) => s_avg_velocity
        );

    --------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk <= '1';
            wait for 5 ns;
            s_clk <= '0';
            wait for 5 ns;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    p_reset : process
    begin
        reset <= '0';
        wait for 48 ns;
        reset <= '1';
        wait for 25 ns;
        reset <= '0';
        wait for 220 ns;
        reset <= '0';
        wait for 17ns;
        reset <= '0';
        wait;
    end process p_reset;

    --------------------------------------------------------------------
    -- Velocity generation process
    --------------------------------------------------------------------
    p_stimulus : process
    begin
        s_velocity <= "0000000";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001100";
        wait for 10 ns;
        s_velocity <= "0001110";
        wait for 10 ns;
        s_velocity <= "0011001";
        wait for 10 ns;
        s_velocity <= "0001000";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001100";
        wait for 10 ns;
        s_velocity <= "0001110";
        wait for 10 ns;
        s_velocity <= "0000000";
        wait for 10 ns;
        s_velocity <= "0001000";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001100";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001001";
        wait for 10 ns;
        s_velocity <= "0001000";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001010";
        wait for 10 ns;
        s_velocity <= "0001100";
        wait for 10 ns;
        s_velocity <= "0001110";
        wait for 10 ns;
        s_velocity <= "0000000";
    end process p_stimulus;

end architecture testbench;

```
![obr1](projekt-avg_vel-waveform-2.PNG) 
## Notes
- Rounding creates some errors. In our application these errors are negligible. 
