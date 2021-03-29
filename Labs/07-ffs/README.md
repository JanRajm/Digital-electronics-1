 ## Cv. 7
 
 ### Preparation task
 | **D** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | Remember |
   | 0 | 1 | 0 | Remember |
   | 1 | 0 | 1 | Sample D |
   | 1 | 1 | 1 | Sample D |

   | **J** | **K** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | 0 | No change |
   | 0 | 0 | 1 | 1 | No change |
   | 0 | 1 | 0 | 0 | Reset |
   | 0 | 1 | 1 | 0 | Reset |
   | 1 | 0 | 0 | 1 | Set |
   | 1 | 0 | 1 | 1 | Set |
   | 1 | 1 | 0 | 1 | Toggle |
   | 1 | 1 | 1 | 0 | Toggle |

   | **T** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | No change |
   | 0 | 1 | 1 | No change |
   | 1 | 0 | 1 | Invert |
   | 1 | 1 | 0 | Invert |

### 2)

### 3)

##### p_d_ff_rst
```vhdl
p_d_ff_rst : process (rst, clk)
    begin
               if (rising_edge(clk)) then
                   if rst = '1' then
                       q     <= '0';
                       q_bar <= '1';
                   elsif (rising_edge(clk)) then
                       q <= d;
                       q_bar <= not d;
                   end if;
               end if;
    end process p_d_ff_rst;
```
##### p_d_ff_arst
```vhdl
p_d_ff_arst : process (arst, clk)
    begin
        if arst = '1' then
            q     <= '0';
            q_bar <= '1';
        elsif (rising_edge(clk)) then
            q <= d;
            q_bar <= not d;
        end if;
    end process p_d_ff_arst;
```
##### p_jk_ff_rst
```vhdl
p_jk_ff_rst : process (rst, clk)
            begin
                if (rising_edge(clk)) then
                    if rst = '1' then
                        s_q <= '0';
                    else
                        if (j = '0' and k = '0') then
                            s_q <= s_q;
                        elsif (j = '0' and k = '1') then
                            s_q <= '0';
                        elsif (j = '1' and k = '0') then
                            s_q <= '1';
                        elsif (j = '1' and k = '1') then
                            s_q <= not s_q;
                        end if;
                    end if;
                end if;
        end process p_jk_ff_rst;
```

##### p_t_ff_rst
```vhdl
p_t_ff_rst : process (rst, clk)
            begin
                if (rising_edge(clk)) then
                    if rst = '1' then
                        s_q <= '0';
                    else
                        if (t = '0' ) then
                            s_q <= s_q;
                        elsif (t = '1') then
                            s_q <= not s_q;
                       
                        end if;
                    end if;
                end if;
        end process p_t_ff_rst;
```
