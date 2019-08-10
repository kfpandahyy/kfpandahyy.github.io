






===================================MySQL5.7
mysqld_main()
	|-init_slave()
	|-int thread_mask= SLAVE_SQL | SLAVE_IO;
	//Create slave info objects by reading repositories of individual channels and add them into channel_map
	|-Rpl_info_factory::create_slave_info_objects(opt_mi_repository_id,opt_rli_repository_id,thread_mask,&channel_map)))
	|-check_slave_sql_config_conflict() //Check if there is any slave SQL config conflict.
	|-if (!opt_skip_slave_start) //如果配置了skip-slave-start，则不会走下面逻辑去启动IO线程和SQL线程
		//Loop through the channel_map and start slave threads for each channel.
		|-for (mi_map::iterator it= channel_map.begin(); it!=channel_map.end(); it++)
			|-start_slave_threads()
				|-if (thread_mask & SLAVE_IO)
					|-is_error=start_slave_thread(...,handle_slave_io,...)
				|-if (!is_error && (thread_mask & SLAVE_SQL))
					//MTS-recovery gaps gathering is placed onto common execution path for either START-SLAVE and --skip-start-slave= 0 
					|-if (mi->rli->recovery_parallel_workers != 0)
						if (mts_recovery_groups(mi->rli))
							|-is_error= true;
					|-if (!is_error)
						|-start_slave_thread(...,handle_slave_sql,...)

mts_recovery_groups()
	|-
