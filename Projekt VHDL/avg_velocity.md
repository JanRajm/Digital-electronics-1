----------------------
-- Made by Jan Rajm --
----------------------
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;               -- Needed for shifts
entity e_avg_velocity is
    Port (
        clk : in std_logic;
        velocity : in std_logic_vector(7-1 downto 0)    
    );
end e_avg_velocity;
 
architecture behave of e_avg_velocity is
  signal s_avg_velocity        : unsigned(7-1 downto 0)  := "0000000";
  signal sum_of_velocities     : unsigned(7-1 downto 0)  := "0000000";
begin
 
p_avg_velocity : process(clk)
-- variable temp is used for clk division by 2. When temp is equal to 0, there is no shift
    --variable sum_of_velocities  : unsigned(7-1 downto 0) := "0000000";
  variable count_of_shifts     : integer                 := 1;
  variable temp                : integer                 := 1;

    begin
    if rising_edge(clk) then
        sum_of_velocities <= sum_of_velocities + unsigned(velocity);    
        temp := 1-temp;
        if temp = 1 then
            s_avg_velocity <= shift_right(unsigned(sum_of_velocities), count_of_shifts);
            count_of_shifts := count_of_shifts;
        end if;
    end if;
 
  end process p_avg_velocity;
end architecture behave;
