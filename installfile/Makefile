bin:=ycc
http_port:="http://127.0.0.1:8921"

run:
	@echo "ycc para starting..."
	@nohup ./$(bin) -f $(bin).toml > /dev/null 2>&1 &
	@echo "done"

show:
	@./$(bin) -f $(bin).toml

stop:
	@./ycc-cli --rpc_laddr=$(http_port) close

unlock:
	@./ycc-cli --rpc_laddr=$(http_port) wallet unlock -p 钱包密码

clear:
	@rm -rf para*
	@rm -rf logs

init:
	@./ycc-cli --rpc_laddr=$(http_port) seed save -s "钱包助记词"  -p 钱包密码
	@make unlock
	@./ycc-cli --rpc_laddr=$(http_port) account import_key -k "账户私钥" -t 2 -l "genness"

