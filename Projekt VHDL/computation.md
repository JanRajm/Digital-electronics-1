## Top module computation

### computation.vhdl
```vhdl
----------------------------------------------------------------------------------
-- Company: BUT
-- Engineer: Jan Rajm
-- 
-- Create Date: 01.05.2021 15:14:37
-- Design Name: computation
-- Module Name: computation - Behavioral
-- Project Name: VHDL school project
-- Target Devices: tachometer
-- Tool Versions: 
-- Description: top module of computation processes
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity computation is
  Port ( 
    status_i   : in std_logic_vector(2-1 downto 0);
    reset_i    : in std_logic;
    clk1hz_i   : in std_logic;
    sensor_i   : in std_logic;
    value_o    : out std_logic_vector(10-1 downto 0)
  );
end computation;

architecture Behavioral of computation is
    signal s_avg_velocity : std_logic_vector(10-1 downto 0);
    signal s_velocity     : std_logic_vector(10-1 downto 0);
    signal s_distance     : std_logic_vector(10-1 downto 0);
    signal s_value        : std_logic_vector(10-1 downto 0);
begin
p_velocity : entity work.velocity
        port map(
        -- submodule => top module
            clk1hz_i   => clk1hz_i,
            sensor_i   => sensor_i,
            velocity_o => s_velocity
        );

p_avg_velocity : entity work.e_avg_velocity 
        port map(
            clk1hz_i       => clk1hz_i,
            reset_i        => reset_i,
            vel2avg_i      => s_velocity,
            avg_velocity_o => s_avg_velocity
        );
        
p_distance : entity work.distance 
        port map(
            sensor_i   => sensor_i,
            reset_i    => reset_i,
            distance_o => s_distance
        );
        
p_output : entity work.output 
        port map(
            clk1hz_i       => clk1hz_i,
            status_i       => status_i,
            distance_i     => s_distance,
            avg_velocity_i => s_avg_velocity,
            velocity_i     => s_velocity,
            value_o        => s_value
        );
end Behavioral;

```
### tb_computation.vhdl
```vhdl
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 01.05.2021 16:06:11
-- Design Name: 
-- Module Name: tb_computation - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity tb_computation is
--  Port ( );
end tb_computation;

architecture Behavioral of tb_computation is
    signal s_status : std_logic_vector(2-1 downto 0);
    signal s_reset  : std_logic;
    signal s_clk    : std_logic;
    signal s_sensor : std_logic;
    signal s_value  : std_logic_vector(10-1 downto 0);
begin
uut_computation: entity work.computation
        port map (
            status_i => s_status,
            reset_i  => s_reset,
            clk1hz_i => s_clk,
            sensor_i => s_sensor,
            value_o  => s_value
        );

p_reset_gen : process
    begin
        s_reset <= '0';
        wait for 7128 ms; -- reset during avarage velocity
        s_reset <= '1';
        wait for 53 ms;
        s_reset <= '0';
        wait for 4128 ms; -- reset during distance
        s_reset <= '1';
        wait for 53 ms;
        s_reset <= '0';
        wait;
    end process p_reset_gen;
    
p_status : process
    begin
        s_status <= "00"; -- velocity
        wait for 4000 ms;
        s_status <= "01"; -- avg. velocity
        wait for 4000 ms;
        s_status <= "10";
        wait for 4000 ms; -- distance
        s_status <= "11"; -- intentionaly unexpected value, should display velocity
        wait for 4000 ms;
        wait;
    end process p_status;
    
p_clk1hz : process
    begin
        while now < 16000 ms loop         
            s_clk <= '1';
            wait for 500 ms;
            s_clk <= '0';
            wait for 500 ms;
        end loop;
        wait;
    end process p_clk1hz;
    
p_sensor : process
    begin
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        
        -- slower 300 ms
        
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        
        --
        
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        
        -- 
        
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        
        --
        
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        s_sensor <= '0';
        wait for 10 ms;
        s_sensor <= '1';
        wait for 10 ms;
        
        -- slower 300 ms
        
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        
        --
        
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        s_sensor <= '0';
        wait for 150 ms;
        s_sensor <= '1';
        wait for 150 ms;
        
        -- 
        
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        s_sensor <= '0';
        wait for 50 ms;
        s_sensor <= '1';
        wait for 50 ms;
        
        wait;
end process p_sensor;

end Behavioral;
```
![obr1](projekt-computation) 
- required values are displayed correctly with timing by rising edge of clock signal
- unexpected value of __s_status__ displays __velocity__ as expected
- signal __reset__ sets required values to zero
